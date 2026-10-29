# 8. Decisões arquiteturais (ADRs)

## ADR-001: Monólito modular, com um contexto por módulo
**Contexto:** a equipe é pequena e o sistema tem um único deploy.  
**Decisão:** manter um monólito, organizado em módulos que correspondem aos contextos delimitados, sem microsserviços.  
**Consequências:** menos custo operacional e transações locais simples. Os limites são mantidos por disciplina (portas e eventos), e não pela rede. Um contexto pode virar serviço depois.

## ADR-002: Agregados se referenciam por identificador
**Contexto:** hoje as tabelas se ligam por associações do ORM, e qualquer módulo alcança qualquer outro.  
**Decisão:** um agregado guarda apenas o `id` de outro (por exemplo, `authorId`), nunca o objeto.  
**Consequências:** agregados pequenos e transações curtas. Consultas que cruzam contextos passam por portas ou por modelos de leitura.

## ADR-003: Contadores como projeções, não como atributos do agregado
**Contexto:** `likesCount` e `commentsCount` ficam na tabela de tutoriais e são alterados na mesma transação da curtida ou do comentário.  
**Decisão:** o contador pertence ao Engajamento, na projeção `TutorialStats`, atualizada por eventos.  
**Consequências:** o Catálogo deixa de ser dono de dados que não são dele. Em troca, aceita-se consistência eventual. Como etapa de migração, a projeção pode ser atualizada na **mesma transação** do caso de uso, mantendo o comportamento atual.

## ADR-004: Portas no domínio e na aplicação, adaptadores na infraestrutura
**Contexto:** hoje o service chama Sequelize, `fs`, bcrypt e JWT.  
**Decisão:** as dependências externas ficam atrás de interfaces; as implementações vivem em `infrastructure`.  
**Consequências:** a regra de negócio é testável sem banco, e trocar tecnologia significa escrever um novo adaptador. Pede mais arquivos e uma raiz de composição (`bootstrap`).

## ADR-005: Eventos de domínio em memória (in-process)
**Status:** evoluída pelo ADR-008 (o barramento em memória segue como implementação inicial e para testes).  
**Contexto:** os contextos precisam reagir uns aos outros sem se importar.  
**Decisão:** barramento em memória por trás da porta `EventPublisher`.  
**Consequências:** simples e sem infraestrutura nova. Os eventos não sobrevivem a uma queda do processo; se isso virar um problema, troca-se por um broker, sem mudar o domínio.

## ADR-006: Autorização na aplicação e no domínio; as rotas só autenticam
**Contexto:** o projeto já separa `401` (quem é você), `403` (isso não é seu) e `404` (não existe), nessa lógica de verificação.  
**Decisão:** o middleware de rota responde só `401`; o caso de uso e a política do domínio decidem `404` e `403`, sempre conferindo "existe?" antes de "é seu?".  
**Consequências:** a regra de quem pode o quê vive em um lugar só e vale para qualquer interface (HTTP hoje, outra amanhã). Esconder um botão na tela continua sendo só conveniência de interface.

## ADR-007: Documentação em português, código em inglês
**Contexto:** o vocabulário do negócio é em português; o código do projeto é em inglês.  
**Decisão:** manter os dois, com o dicionário em [linguagem ubíqua](02-linguagem-ubiqua.md).  
**Consequências:** quem fala com o negócio e quem lê o código usam o mesmo mapa de termos.

## ADR-008: Mensageria com RabbitMQ e padrão outbox
**Contexto:** o barramento em memória (ADR-005) não alcança processos separados e perde eventos se a aplicação cair. Há necessidades que pedem processamento fora da requisição: notificações aos membros, processamento de vídeo e capa, e limpeza entre contextos na exclusão de um tutorial.  
**Decisão:** adotar o RabbitMQ (exchange `topic` chamada `craftcorner.events`, uma fila por consumidor e filas de erro). A publicação usa o padrão **outbox**: o evento é gravado no MySQL na mesma transação do agregado e um relay o envia ao broker. O consumo usa confirmação manual, idempotência por `eventId` e reenvio com fila de erro. A porta `EventPublisher` não muda.  
**Alternativas descartadas:** manter só o barramento em memória (não sobrevive à queda e não alcança workers), Kafka (excessivo para o porte do projeto), Redis Pub/Sub (perde mensagens sem consumidor ativo) e serviço gerenciado em nuvem (custo e dependência fora do escopo).  
**Consequências:** desacoplamento, resiliência e possibilidade de escalar os workers. Em troca, há mais uma peça de infraestrutura para operar e a consistência entre contextos é eventual. Detalhes e diagramas C4 em [camada de mensageria](09-camada-de-mensageria.md).
