# Identity & Access / application / ports

Portas de saída: interfaces para o que o caso de uso precisa e não deve conhecer (armazenamento, token, outro contexto).

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| PasswordHasher.js | Gerar e comparar hash de senha. |
| TokenIssuer.js | Emitir e validar o token de acesso. |
| EventPublisher.js | Publicar eventos de domínio. |
