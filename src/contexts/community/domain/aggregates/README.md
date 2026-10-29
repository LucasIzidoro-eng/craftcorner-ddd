# Community / domain / aggregates

Agregados. Cada um tem uma raiz, que é a única porta de entrada para alterar seu conteúdo, e define a fronteira de consistência e de transação. Agregados se referenciam por identificador, nunca por objeto.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| Profile.js | Raiz do perfil (o id é o mesmo da conta, referenciado por UserId). |
| FollowRelation.js | Raiz da relação de seguir: o par seguidor e seguido é único e não permite seguir a si mesmo. |
