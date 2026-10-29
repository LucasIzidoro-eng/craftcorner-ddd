# Community / infrastructure / persistence

Adaptadores de persistência: implementam os repositórios e fazem o mapeamento entre tabela e agregado.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| SequelizeProfileRepository.js | Implementa ProfileRepository. |
| SequelizeFollowRepository.js | Implementa FollowRepository (índice único no par). |
| ProfileMapper.js | Mapeia tabela e agregado. |
