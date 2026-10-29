# Identity & Access / domain / value-objects

Objetos de valor: imutáveis, comparados pelo conteúdo, e que validam a si mesmos ao serem criados.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| Email.js | Formato válido e normalizado. |
| Username.js | Entre 3 e 20 caracteres, com trim aplicado antes de validar. |
| PasswordHash.js | Encapsula o hash; nunca expõe a senha original. |
| Role.js | Membro ou Admin. |
| AccountStatus.js | Ativa ou Suspensa. |
