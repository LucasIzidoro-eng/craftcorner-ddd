# Roteiro de apresentação (cerca de 5 minutos)

## 1. O domínio (30 s)
Ler o parágrafo do [README](../README.md). Ideia central: o CraftCorner **ensina e conecta praticantes de artesanato**.

## 2. Subdomínios (45 s)
Mostrar o diagrama de [subdomínios](01-dominio-e-subdominios.md). O **Core** é ensinar e praticar (Catálogo de Tutoriais e Aprendizado e Criações). **Identidade e Acesso é genérico**: usa solução padrão, sem investir em modelagem própria.

## 3. Contextos e mapa de contextos (90 s)
Mostrar o [mapa de contextos](03-contextos-delimitados-e-context-map.md). Pontos para falar:
- Sete contextos, cada um com seu modelo e vocabulário.
- Identity & Access publica só `UserId` e papel (Open Host Service); os outros se conformam.
- Entre contextos há dois jeitos de conversar: **consulta por porta** (camada anticorrupção) e **evento**.

## 4. Modelo tático (60 s)
Abrir o agregado `Tutorial` ([modelo tático](04-modelo-tatico.md)):
- A raiz protege as invariantes (título com `trim` antes de `notEmpty`, só o autor edita, o vídeo não é trocado depois de publicado).
- Referencia o autor por `authorId`.
- Contadores ficam fora: são a projeção `TutorialStats`.

## 5. Camadas e regra de dependência (45 s)
Mostrar o diagrama de [camadas](05-arquitetura-em-camadas.md): setas para dentro; o domínio não importa Express nem Sequelize; a infraestrutura **implementa** as portas.

## 5.1 DDD e hexagonal juntos (20 s)
Mostrar o diagrama hexagonal do [README](../README.md): o núcleo (domínio e casos de uso) não conhece tecnologia; as **portas** são interfaces e os **adaptadores** (MySQL, disco, JWT, HTTP) as implementam.

## 6. Do projeto atual para o DDD (45 s)
Mostrar a tabela de [diagnóstico](07-mapeamento-projeto-atual.md): `tutorialService` importa `Like` e `Comment` para excluir → vira o evento `TutorialDeleted`; `associations.js` desaparece → referência por identificador.

## 7. Fechamento (15 s)
Os módulos que já existem apontam para os contextos; o caminho de migração é incremental ([roteiro de migração](07-mapeamento-projeto-atual.md)).

## Perguntas prováveis e respostas curtas

**Por que não um microsserviço por contexto?**  
Um único deploy basta e custa menos. Os limites ficam claros pelas portas e eventos, e um contexto pode sair depois ([ADR-001](08-decisoes-arquiteturais.md)).

**Por que referenciar por identificador, e não por objeto?**  
Mantém os agregados pequenos e as transações curtas, e impede que um contexto dependa das tabelas de outro ([ADR-002](08-decisoes-arquiteturais.md)).

**O contador de curtidas não deveria estar no tutorial?**  
Ele é uma métrica do Engajamento. Guardá-lo no tutorial faz o Catálogo depender de dados que não são dele. Por isso é uma projeção atualizada por eventos ([ADR-003](08-decisoes-arquiteturais.md)).

**O que muda na prática para quem programa?**  
A regra de negócio vira código puro, testável sem banco, e trocar MySQL ou disco significa escrever um adaptador novo.

**O que já existe e o que é planejado?**  
Identidade, perfil, tutoriais, curtidas, comentários, feed Geral e busca já existem no projeto. Seguir, avaliar, categorias, criações, projetos concluídos, feed Seguindo e moderação estão previstos e já têm lugar na arquitetura.
