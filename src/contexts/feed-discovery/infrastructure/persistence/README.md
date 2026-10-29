# Feed & Discovery / infrastructure / persistence

Adaptadores de persistência: implementam os repositórios e fazem o mapeamento entre tabela e agregado.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| SqlFeedReadStore.js | Implementa FeedReadStore. No início, consultas sobre as tabelas do Catálogo; depois, tabelas de leitura próprias. |
