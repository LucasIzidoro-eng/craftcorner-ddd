# Identity & Access / infrastructure / persistence

Adaptadores de persistência: implementam os repositórios e fazem o mapeamento entre tabela e agregado.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| SequelizeUserRepository.js | Implementa UserRepository sobre MySQL. |
| UserMapper.js | Converte entre a tabela users e o agregado User. |
