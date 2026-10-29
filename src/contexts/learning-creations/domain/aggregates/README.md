# Learning & Creations / domain / aggregates

Agregados. Cada um tem uma raiz, que é a única porta de entrada para alterar seu conteúdo, e define a fronteira de consistência e de transação. Agregados se referenciam por identificador, nunca por objeto.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| ProjectProgress.js | Raiz do progresso (projeto concluído). |
| Creation.js | Raiz da criação compartilhada. |
