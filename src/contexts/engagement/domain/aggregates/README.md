# Engagement / domain / aggregates

Agregados. Cada um tem uma raiz, que é a única porta de entrada para alterar seu conteúdo, e define a fronteira de consistência e de transação. Agregados se referenciam por identificador, nunca por objeto.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| Like.js | Raiz da curtida; referencia o alvo por identificador. |
| Comment.js | Raiz do comentário. |
| Rating.js | Raiz da avaliação. |
