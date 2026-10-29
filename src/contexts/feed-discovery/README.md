# Feed & Discovery — Feed e Descoberta

**Tipo de subdomínio:** Suporte  
**Situação no projeto atual:** 🟡 Parcial

## Responsabilidade
Lado de leitura da plataforma: feed Geral, feed Seguindo, exploração por categoria e busca. Não tem agregados, apenas modelos de leitura montados a partir de eventos.

## O que este contexto **não** faz
Não altera tutoriais, curtidas nem relações de seguir. Só lê.

## Linguagem ubíqua do contexto
- Feed Geral
- Feed Seguindo
- Busca
- Exploração por categoria
- Página (page e limit)

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** —  
**Consultas:** `GetGeneralFeed`, `GetFollowingFeed`, `SearchTutorials`, `BrowseByCategory`

## Eventos de domínio
**Publica:**
- Nenhum (contexto de leitura).

**Consome:**
- TutorialPublished
- TutorialDeleted
- TutorialHidden
- TutorialRestored
- CreationPosted
- UserFollowed
- UserUnfollowed

## Relações no mapa de contextos
- **Upstream (de quem depende):** Tutorial Catalog, Community, Learning & Creations, Engagement
- **Downstream (quem depende dele):** Interface web

## De onde vem no código atual
`GET /feed` (rota em `modules/user`) e `modules/search`; feed Seguindo ainda não implementado

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
