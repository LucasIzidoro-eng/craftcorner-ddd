# Tutorial Catalog / application / ports

Portas de saída: interfaces para o que o caso de uso precisa e não deve conhecer (armazenamento, token, outro contexto).

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| MediaStorage.js | Salvar e apagar capa e vídeo. |
| TutorialStatsReader.js | Lê curtidas, comentários e visualizações do Engajamento. |
| UnitOfWork.js | Fronteira de transação. |
| EventPublisher.js | Publicar eventos. |
