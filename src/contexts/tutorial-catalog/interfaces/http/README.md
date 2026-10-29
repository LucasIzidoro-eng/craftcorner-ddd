# Tutorial Catalog / interfaces / http

Adaptador de entrada REST: rotas, controllers, validadores de entrada e middlewares.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| tutorialRoutes.js | Upload, detalhe, stream, edição, exclusão e meus tutoriais. |
| TutorialController.js | Controller magro. |
| tutorialValidators.js | Validação de entrada (trim antes de notEmpty). |
| tutorialUploadMiddleware.js | Multer: .fields para publicar, .single para trocar a capa. |
| categoryRoutes.js | Gerenciamento de categorias (requireAdmin). |
