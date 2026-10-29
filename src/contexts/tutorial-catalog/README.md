# Tutorial Catalog — Catálogo de Tutoriais

**Tipo de subdomínio:** Core  
**Situação no projeto atual:** ✅ Implementado

## Responsabilidade
Publicação, edição, exclusão e consulta de tutoriais (imagem de capa e vídeo), organizados por categoria (tipo de artesanato).

## O que este contexto **não** faz
Não conta curtidas nem comentários (Engajamento) e não decide quem é moderador (Administração). Não sabe nada de senha.

## Linguagem ubíqua do contexto
- Tutorial
- Categoria (tipo de artesanato)
- Criador (autor)
- Capa (CoverImage)
- Vídeo do tutorial
- Publicado ou Oculto
- Visualização

## Interface pública (o que os outros contextos podem pedir)
**Comandos:** `PublishTutorial`, `ChangeTutorialDetails`, `ReplaceTutorialCover`, `DeleteTutorial`, `CreateCategory`, `RenameCategory`  
**Consultas:** `GetTutorialDetails`, `GetTutorialForEdit`, `ListMyTutorials`, `ListTutorialsByCategory`, `StreamTutorialVideo`

## Eventos de domínio
**Publica:**
- `TutorialPublished`
- `TutorialDetailsChanged`
- `TutorialCoverReplaced`
- `TutorialDeleted`
- `TutorialHidden`
- `TutorialRestored`
- `CategoryCreated`

**Consome:**
- ContentHidden (Moderação)
- ContentRestored (Moderação)

## Relações no mapa de contextos
- **Upstream (de quem depende):** Identity & Access (UserId, papel)
- **Downstream (quem depende dele):** Engagement, Learning & Creations, Feed & Discovery

## De onde vem no código atual
`modules/tutorial` (upload, detalhe, edição, exclusão, stream), `middlewares/tutorialMulter.js`; categorias ainda não implementadas

## Camadas deste contexto
- [`domain/`](domain) — Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.
- [`application/`](application) — Casos de uso: orquestram o domínio e usam portas. Não conhecem HTTP nem SQL.
- [`infrastructure/`](infrastructure) — Adaptadores de saída: implementam as portas com tecnologia concreta (MySQL, disco, bcrypt, JWT).
- [`interfaces/`](interfaces) — Adaptadores de entrada: traduzem o mundo externo (HTTP) em chamadas aos casos de uso.
