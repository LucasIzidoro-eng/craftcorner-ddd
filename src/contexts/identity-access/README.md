# Identity & Access — Identidade e Acesso

**Tipo de subdomínio:** Genérico  
**Situação no projeto atual:** ✅ Implementado

## Responsabilidade
Cadastro de contas, autenticação e papéis (Membro ou Administrador). Responde à pergunta "quem é você e o que pode fazer?".

## O que este contexto **não** faz
Não guarda biografia, foto de perfil nem seguidores (isso é do contexto Comunidade). Não conhece tutoriais.

## Linguagem ubíqua do contexto
- Conta (User)
- E-mail
- Nome de usuário (Username)
- Senha (PasswordHash)
- Papel (Role: Membro ou Admin)
- Token de acesso

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** `RegisterUser`, `AuthenticateUser`  
**Consultas:** `GetAccountSummary`

## Eventos de domínio
**Publica:**
- `UserRegistered`

**Consome:**
- UserSuspended (Moderação)
- UserReactivated (Moderação)

## Relações no mapa de contextos
- **Upstream (de quem depende):** —
- **Downstream (quem depende dele):** Todos os outros contextos (recebem apenas UserId e papel)

## De onde vem no código atual
`modules/user` (register, login), `middlewares/auth.js`, `middlewares/optionalAuth.js`, `config/jwt.js`

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
