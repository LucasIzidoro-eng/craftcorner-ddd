# Engagement / application / ports

Portas de saída: interfaces para o que o caso de uso precisa e não deve conhecer (armazenamento, token, outro contexto).

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| TutorialLookup.js | Camada anticorrupção: o tutorial existe e está publicado? |
| UnitOfWork.js | Fronteira de transação. |
| EventPublisher.js | Publicar eventos. |
