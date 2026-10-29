# Community / application / ports

Portas de saída: interfaces para o que o caso de uso precisa e não deve conhecer (armazenamento, token, outro contexto).

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| TutorialCatalogReader.js | Camada anticorrupção: lista os tutoriais de um autor sem acessar tabelas do Catálogo. |
| AvatarStorage.js | Salvar e apagar a foto de perfil. |
| EventPublisher.js | Publicar eventos. |
