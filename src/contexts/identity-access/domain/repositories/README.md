# Identity & Access / domain / repositories

Portas de persistência dos agregados. Só a interface mora aqui; a implementação fica em infrastructure.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| UserRepository.js | Porta: findById, findByEmailOrUsername, existsByEmail, existsByUsername, save. |
