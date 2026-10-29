# Identity & Access / interfaces / http

Adaptador de entrada REST: rotas, controllers, validadores de entrada e middlewares.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| authRoutes.js | POST /api/register e POST /api/login. |
| AuthController.js | Traduz HTTP em comandos de aplicação. |
| authValidators.js | Validação de entrada (trim antes de notEmpty). |
| isAuthenticated.js | Middleware: sem token válido, responde 401. |
| optionalAuth.js | Middleware: identifica quem pede, mas nunca bloqueia. |
| requireAdmin.js | Middleware: exige papel Admin (403 se não for). |
