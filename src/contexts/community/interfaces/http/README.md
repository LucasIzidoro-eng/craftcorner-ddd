# Community / interfaces / http

Adaptador de entrada REST: rotas, controllers, validadores de entrada e middlewares.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| profileRoutes.js | GET e PUT /api/profile/me e GET /api/profile/:username (optionalAuth, depois de /profile/me). |
| ProfileController.js | Controller magro. |
| profileValidators.js | Validação de entrada. |
| avatarUploadMiddleware.js | Multer de campo único. |
