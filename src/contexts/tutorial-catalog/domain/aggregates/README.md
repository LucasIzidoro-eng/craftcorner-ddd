# Tutorial Catalog / domain / aggregates

Agregados. Cada um tem uma raiz, que é a única porta de entrada para alterar seu conteúdo, e define a fronteira de consistência e de transação. Agregados se referenciam por identificador, nunca por objeto.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| Tutorial.js | Raiz. Guarda authorId e categoryId apenas como identificadores. Métodos: publish, changeDetails, replaceCover, hide, restore e canBeManagedBy. |
| Category.js | Raiz da categoria (tipo de artesanato). |
