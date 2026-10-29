# Identity & Access / application / commands

Casos de uso que alteram estado. Um arquivo por caso de uso.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| RegisterUser.js | Valida a política de senha, cria o User e publica UserRegistered. |
| AuthenticateUser.js | Confere as credenciais e pede um token ao TokenIssuer. |
