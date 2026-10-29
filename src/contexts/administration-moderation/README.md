# Administration & Moderation — Administração e Moderação

**Tipo de subdomínio:** Suporte  
**Situação no projeto atual:** 🕒 Planejado

## Responsabilidade
Dashboard com estatísticas e moderação de conteúdos e usuários. As decisões viram eventos que os outros contextos obedecem.

## O que este contexto **não** faz
Não edita o conteúdo dos outros contextos: decide e publica um evento. Categorias são gerenciadas no Catálogo (com papel Admin).

## Linguagem ubíqua do contexto
- Administrador (Admin)
- Moderação (ModerationAction)
- Ocultar e restaurar conteúdo
- Suspender e reativar usuário
- Dashboard
- Motivo da moderação

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** `HideContent`, `RestoreContent`, `SuspendUser`, `ReactivateUser`  
**Consultas:** `GetDashboardStatistics`, `ListModerationHistory`, `ListUsersForModeration`

## Eventos de domínio
**Publica:**
- `ContentHidden`
- `ContentRestored`
- `UserSuspended`
- `UserReactivated`

**Consome:**
- UserRegistered
- TutorialPublished
- TutorialDeleted
- CommentAdded
- TutorialLiked

## Relações no mapa de contextos
- **Upstream (de quem depende):** Identity & Access (papel Admin)
- **Downstream (quem depende dele):** Tutorial Catalog, Engagement, Identity & Access (obedecem às decisões)

## De onde vem no código atual
Telas de administração ainda como espaço reservado; `isAdmin` já existe no token

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
