# Engagement — Engajamento (Curtidas, Comentários, Avaliações e Métricas)

**Tipo de subdomínio:** Suporte  
**Situação no projeto atual:** 🟡 Parcial

## Responsabilidade
Interações das pessoas com o conteúdo: curtir, comentar, avaliar e contar visualizações. Mantém as estatísticas de cada tutorial.

## O que este contexto **não** faz
Não cria nem edita tutoriais. Apenas consulta se o alvo existe, por uma porta.

## Linguagem ubíqua do contexto
- Curtida (Like)
- Comentário (Comment)
- Avaliação (Rating)
- Estrelas (Stars)
- Alvo (LikeTarget)
- Estatísticas do tutorial (TutorialStats)

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** `ToggleLike`, `AddComment`, `RemoveComment`, `RateTutorial`, `RegisterTutorialView`  
**Consultas:** `CheckLikeStatus`, `ListLikedTutorials`, `ListComments`, `GetTutorialStats`

## Eventos de domínio
**Publica:**
- `TutorialLiked`
- `TutorialUnliked`
- `CommentAdded`
- `CommentRemoved`
- `TutorialRated`
- `TutorialViewed`

**Consome:**
- TutorialDeleted (Catálogo)
- ContentHidden (Moderação)
- UserSuspended (Moderação)

## Relações no mapa de contextos
- **Upstream (de quem depende):** Tutorial Catalog (existência do alvo), Identity & Access
- **Downstream (quem depende dele):** Feed & Discovery, Administração

## De onde vem no código atual
`modules/like`, `modules/comment`; contadores `likesCount`, `commentsCount` e `views` na tabela de tutoriais; avaliação ainda não implementada

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
