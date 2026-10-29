# Feed & Discovery / application / event-handlers

Reações a eventos de domínio (do próprio contexto ou de outros).

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| AddToFeedWhenPublished.js | Reage a TutorialPublished e CreationPosted. |
| RemoveFromFeedWhenDeletedOrHidden.js | Reage a TutorialDeleted, TutorialHidden e TutorialRestored. |
| RebuildFollowingWhenFollowChanges.js | Reage a UserFollowed e UserUnfollowed. |
