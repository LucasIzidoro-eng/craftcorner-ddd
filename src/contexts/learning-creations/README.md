# Learning & Creations — Aprendizado e Criações

**Tipo de subdomínio:** Core  
**Situação no projeto atual:** 🕒 Planejado

## Responsabilidade
O progresso de quem aprende (marcar projetos concluídos) e as fotos das criações pessoais compartilhadas com a comunidade.

## O que este contexto **não** faz
Não publica tutoriais (Catálogo) e não conta curtidas (Engajamento).

## Linguagem ubíqua do contexto
- Projeto concluído (ProjectProgress)
- Criação (Creation)
- Foto da criação
- Legenda (Caption)
- Inspirada em um tutorial

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** `MarkProjectCompleted`, `UnmarkProjectCompleted`, `PostCreation`, `DeleteCreation`  
**Consultas:** `ListCompletedProjects`, `ListCreationsByUser`, `GetCreation`

## Eventos de domínio
**Publica:**
- `ProjectCompleted`
- `ProjectCompletionRevoked`
- `CreationPosted`
- `CreationDeleted`

**Consome:**
- TutorialDeleted (Catálogo)
- UserSuspended (Moderação)

## Relações no mapa de contextos
- **Upstream (de quem depende):** Tutorial Catalog, Identity & Access
- **Downstream (quem depende dele):** Feed & Discovery, Engagement (curtir criações)

## De onde vem no código atual
Ainda não implementado (previsto na descrição do projeto)

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
