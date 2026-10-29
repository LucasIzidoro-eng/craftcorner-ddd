# Tutorial Catalog / domain / repositories

Portas de persistência dos agregados. Só a interface mora aqui; a implementação fica em infrastructure.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| TutorialRepository.js | Porta: findById, save, delete, listByAuthor, listByCategory. |
| CategoryRepository.js | Porta de persistência de Category. |
