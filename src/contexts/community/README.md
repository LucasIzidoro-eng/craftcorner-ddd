# Community — Comunidade (Perfis e Seguidores)

**Tipo de subdomínio:** Suporte  
**Situação no projeto atual:** 🟡 Parcial

## Responsabilidade
Perfil público das pessoas e a relação de seguir e deixar de seguir criadores.

## O que este contexto **não** faz
Não autentica ninguém e não publica tutoriais. A lista de tutoriais exibida no perfil é obtida do Catálogo por uma porta.

## Linguagem ubíqua do contexto
- Perfil (Profile)
- Biografia (Bio)
- Foto de perfil (Avatar)
- Seguir (Follow)
- Seguidor (Follower)
- Seguindo (Following)
- Criador

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** `UpdateProfile`, `FollowUser`, `UnfollowUser`  
**Consultas:** `GetPublicProfile`, `GetMyProfile`, `ListFollowers`, `ListFollowing`

## Eventos de domínio
**Publica:**
- `ProfileUpdated`
- `UserFollowed`
- `UserUnfollowed`

**Consome:**
- UserRegistered (Identidade)

## Relações no mapa de contextos
- **Upstream (de quem depende):** Identity & Access (UserId)
- **Downstream (quem depende dele):** Feed & Discovery

## De onde vem no código atual
`modules/user` (profile/me, profile/:username); seguir ainda não implementado

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
