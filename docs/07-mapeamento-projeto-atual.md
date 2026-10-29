# 7. Do projeto atual para a arquitetura DDD

## 7.1 Como o projeto é hoje

Uma API Node/Express com MySQL (Sequelize) e um frontend Vue 3. A API é organizada em **módulos por funcionalidade** (`user`, `tutorial`, `like`, `comment`, `search`), cada um em camadas **Route → Controller → Service → Model**. É uma boa base: os módulos já apontam para os futuros contextos. O que falta é separar regra de negócio de tecnologia e desacoplar os módulos entre si.

## 7.2 Mapeamento dos módulos

| Hoje | Vira | Contexto |
|---|---|---|
| `modules/user` (register e login), `middlewares/auth.js`, `middlewares/optionalAuth.js`, `config/jwt.js` | Casos `RegisterUser` e `AuthenticateUser`; adaptadores `JwtTokenIssuer` e `BcryptPasswordHasher`; middlewares em `interfaces/http` | Identity & Access |
| `modules/user` (profile/me e profile/:username) | Casos `UpdateProfile` e `GetPublicProfile` | Community |
| `modules/tutorial` | Agregado `Tutorial` e casos de uso de publicar, editar, excluir e consultar | Tutorial Catalog |
| `middlewares/tutorialMulter.js` | Adaptador `LocalDiskMediaStorage` e middleware de upload em `interfaces/http` | Tutorial Catalog |
| `modules/like` e `modules/comment` | Agregados `Like` e `Comment` e a projeção `TutorialStats` | Engagement |
| `GET /feed` (hoje em `modules/user`) e `modules/search` | Consultas `GetGeneralFeed` e `SearchTutorials` | Feed & Discovery |
| `middlewares/errorHandler.js` | `bootstrap/http/errorHandler.js` (traduz erros de domínio e limpa arquivos órfãos) | Transversal |
| `config/associations.js` | **Removido**: agregados se referenciam por identificador | — |
| Colunas `likesCount`, `commentsCount` e `views` na tabela de tutoriais | Projeção `TutorialStats` | Engagement |

## 7.3 Diagnóstico: o que a nova arquitetura resolve

| Situação atual | Problema | Solução DDD |
|---|---|---|
| `tutorialService` importa os models `Like` e `Comment` para apagar dados na exclusão. | O Catálogo conhece detalhes do Engajamento (acoplamento entre módulos). | O Catálogo só publica `TutorialDeleted`; o Engajamento limpa o que é dele. |
| `userService.getPublicProfile` importa o model `Tutorial`. | Comunidade depende da tabela do Catálogo. | Porta `TutorialCatalogReader` (camada anticorrupção). |
| Contadores guardados na tabela de tutoriais e alterados junto com a linha de curtida. | O Catálogo é dono de dados que não são dele. | Projeção `TutorialStats` do Engajamento, atualizada por eventos. |
| Regras (`trim`, limites de tamanho, "só o autor edita") espalhadas entre validadores, services e controllers. | A mesma regra pode ser aplicada de jeitos diferentes. | Objetos de valor (`Title`, `CommentContent`) e agregados concentram as invariantes. |
| `associations.js` liga todos os models entre si. | Qualquer módulo alcança qualquer tabela. | Referência por identificador e consultas por portas. |
| O service chama Sequelize e `fs` diretamente. | Não dá para testar a regra sem banco e sem disco. | Portas (`TutorialRepository`, `MediaStorage`) com implementações trocáveis, e fakes nos testes. |

## 7.4 Roteiro de migração (incremental, sem reescrever tudo)

1. **Agrupar por contexto:** mover cada módulo atual para `src/contexts/<contexto>` sem mudar o comportamento.
2. **Extrair objetos de valor e agregados** do que hoje são validadores e services (`Title`, `CommentContent`, `Tutorial`).
3. **Criar as portas** (`TutorialRepository`, `MediaStorage`, `PasswordHasher`) e mover o código de Sequelize, `fs`, bcrypt e JWT para `infrastructure`.
4. **Trocar os imports entre módulos** por portas de consulta (ACL) e por eventos (`TutorialDeleted`), eliminando `associations.js`.
5. **Mover os contadores para a projeção** `TutorialStats`, primeiro na mesma transação e depois de forma eventual.
6. **Implementar o que está planejado** (seguir, avaliar, categorias, criações, moderação) já dentro da nova estrutura.
