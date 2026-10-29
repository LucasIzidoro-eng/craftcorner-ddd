# Identity & Access / domain / aggregates

Agregados. Cada um tem uma raiz, que é a única porta de entrada para alterar seu conteúdo, e define a fronteira de consistência e de transação. Agregados se referenciam por identificador, nunca por objeto.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| User.js | Raiz do agregado de conta. Aplica as invariantes: unicidade (via UserRepository), senha só como hash e conta suspensa não autentica. |
