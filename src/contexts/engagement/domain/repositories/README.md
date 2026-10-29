# Engagement / domain / repositories

Portas de persistência dos agregados. Só a interface mora aqui; a implementação fica em infrastructure.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| LikeRepository.js | Porta: findByUserAndTarget, save, remove, listByUser. |
| CommentRepository.js | Porta de persistência de Comment. |
| RatingRepository.js | Porta de persistência de Rating. |
