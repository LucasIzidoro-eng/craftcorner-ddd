# Shared Kernel (núcleo compartilhado)

Parte **mínima e estável** do modelo que todos os contextos aceitam compartilhar. Qualquer mudança aqui exige acordo entre os contextos, por isso ele é pequeno de propósito.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| `ids/UserId.js` | Identificador de usuário. É a única informação de identidade que os outros contextos conhecem. |
| `ids/TutorialId.js` | Identificador de tutorial. |
| `DomainEvent.js` | Base dos eventos: nome, `occurredAt` e identificador do agregado. |
| `EventPublisher.js` | Porta para publicar eventos (implementada pelo barramento em `bootstrap`). |
| `errors/DomainError.js` | Erro-base de regra de negócio. |
| `errors/ValidationError.js` | Entrada inválida (HTTP 400). |
| `errors/ForbiddenError.js` | Logado, mas sem permissão (HTTP 403). |
| `errors/NotFoundError.js` | O recurso não existe (HTTP 404). |
| `Page.js` | Paginação (`page` e `limit`, com a conta `offset = (page - 1) * limit`). |
| `Clock.js` | Porta para a hora atual (facilita testes). |

## O que **não** entra aqui
Entidades e regras de um contexto específico. Se algo só interessa a um contexto, ele fica nesse contexto.
