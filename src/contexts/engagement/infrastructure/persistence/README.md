# Engagement / infrastructure / persistence

Adaptadores de persistência: implementam os repositórios e fazem o mapeamento entre tabela e agregado.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| SequelizeLikeRepository.js | Implementa LikeRepository com índice único composto (usuário e alvo). |
| SequelizeCommentRepository.js | Implementa CommentRepository. |
| SequelizeRatingRepository.js | Implementa RatingRepository. |
| TutorialStatsStore.js | Guarda a projeção de estatísticas. |
