# 6. Eventos de domínio

Eventos são **fatos que já aconteceram**, nomeados no passado. É por eles que os contextos se mantêm sincronizados **sem se conhecerem**.

## 6.1 Catálogo de eventos

A coluna **Situação da funcionalidade** indica se a funcionalidade que origina o evento já existe no projeto atual. O projeto atual ainda não publica eventos: a publicação faz parte do redesenho.

| Evento | Publicado por | Dados principais | Consumido por | Finalidade | Situação da funcionalidade |
|---|---|---|---|---|---|
| `UserRegistered` | Identity & Access | userId, username, occurredAt | Community, Administration & Moderation | Criar o perfil e atualizar estatísticas | ✅ Implementado |
| `ProfileUpdated` | Community | userId | Feed & Discovery | Atualizar nome e foto exibidos nos cards | ✅ Implementado |
| `UserFollowed` | Community | followerId, followedId | Feed & Discovery, Community (contadores) | Montar o feed Seguindo | 🕒 Planejado |
| `UserUnfollowed` | Community | followerId, followedId | Feed & Discovery, Community (contadores) | Retirar itens do feed Seguindo | 🕒 Planejado |
| `TutorialPublished` | Tutorial Catalog | tutorialId, authorId, categoryId, title | Feed & Discovery, Administration & Moderation | Entrar nos feeds e na busca | ✅ Implementado |
| `TutorialDetailsChanged` | Tutorial Catalog | tutorialId, title | Feed & Discovery | Atualizar título nos feeds e na busca | ✅ Implementado |
| `TutorialCoverReplaced` | Tutorial Catalog | tutorialId, coverFile | Feed & Discovery | Atualizar a capa nos cards | ✅ Implementado |
| `TutorialDeleted` | Tutorial Catalog | tutorialId, authorId | Engagement, Learning & Creations, Feed & Discovery | Cada contexto limpa seus próprios dados | ✅ Implementado |
| `TutorialHidden` | Tutorial Catalog | tutorialId | Feed & Discovery, Engagement | Retirar dos feeds | 🕒 Planejado |
| `TutorialRestored` | Tutorial Catalog | tutorialId | Feed & Discovery, Engagement | Voltar aos feeds | 🕒 Planejado |
| `CategoryCreated` | Tutorial Catalog | categoryId, name | Feed & Discovery | Habilitar a exploração por categoria | 🕒 Planejado |
| `MediaProcessed` | Tutorial Catalog (Worker de Mídia) | tutorialId, thumbnailFile, status | Tutorial Catalog, Feed & Discovery | Marcar o vídeo como pronto e atualizar a miniatura nos cards | 🕒 Planejado |
| `TutorialLiked` | Engagement | userId, tutorialId | Engagement (TutorialStats), Feed & Discovery, Administration & Moderation | Aumentar likesCount | ✅ Implementado |
| `TutorialUnliked` | Engagement | userId, tutorialId | Engagement (TutorialStats), Feed & Discovery | Diminuir likesCount | ✅ Implementado |
| `CommentAdded` | Engagement | commentId, tutorialId, authorId | Engagement (TutorialStats), Administration & Moderation | Aumentar commentsCount | ✅ Implementado |
| `CommentRemoved` | Engagement | commentId, tutorialId | Engagement (TutorialStats) | Diminuir commentsCount | 🕒 Planejado |
| `TutorialRated` | Engagement | userId, tutorialId, stars | Engagement (TutorialStats) | Recalcular a média de estrelas | 🕒 Planejado |
| `TutorialViewed` | Engagement | tutorialId, viewerId (opcional) | Engagement (TutorialStats) | Aumentar viewsCount | ✅ Implementado |
| `ProjectCompleted` | Learning & Creations | userId, tutorialId | Administration & Moderation (estatísticas) | Registrar o projeto concluído | 🕒 Planejado |
| `ProjectCompletionRevoked` | Learning & Creations | userId, tutorialId | Administration & Moderation (estatísticas) | Desfazer o registro | 🕒 Planejado |
| `CreationPosted` | Learning & Creations | creationId, authorId, inspiredByTutorialId | Feed & Discovery | Entrar nos feeds | 🕒 Planejado |
| `CreationDeleted` | Learning & Creations | creationId | Feed & Discovery, Engagement | Retirar dos feeds e das curtidas | 🕒 Planejado |
| `ContentHidden` | Administration & Moderation | target, reason, moderatorId | Tutorial Catalog, Engagement, Learning & Creations, Feed & Discovery | Cada contexto oculta o seu conteúdo | 🕒 Planejado |
| `ContentRestored` | Administration & Moderation | target, moderatorId | Tutorial Catalog, Engagement, Learning & Creations, Feed & Discovery | Cada contexto restaura o seu conteúdo | 🕒 Planejado |
| `UserSuspended` | Administration & Moderation | userId, reason, moderatorId | Identity & Access, Engagement, Learning & Creations | Bloquear a conta e esconder as interações | 🕒 Planejado |
| `UserReactivated` | Administration & Moderation | userId, moderatorId | Identity & Access | Reativar a conta | 🕒 Planejado |


Todo evento carrega `eventId`, `occurredAt` e o identificador do agregado (definidos em `shared-kernel/DomainEvent.js`).

## 6.2 Como os eventos circulam

1. O caso de uso altera **um** agregado e o salva.
2. O caso de uso publica o evento pela porta `EventPublisher`.
3. O barramento (`bootstrap/InProcessEventBus.js`) entrega o evento aos handlers inscritos, de outros contextos.
4. Cada handler atualiza apenas **o seu** contexto (uma projeção, uma limpeza, um feed).

Por ser em memória, o barramento é simples e rápido, e é a implementação inicial (e a usada nos testes). Para entregar eventos a processos separados e não perdê-los se a aplicação cair, ele evolui para um message broker (RabbitMQ) com o padrão outbox, sem mudar nenhum agregado ou caso de uso, porque eles só conhecem `EventPublisher`. Ver [ADR-008](08-decisoes-arquiteturais.md) e a [camada de mensageria](09-camada-de-mensageria.md).

## 6.3 Consistência

- **Dentro** de um agregado: consistência imediata (uma transação).
- **Entre** contextos: consistência **eventual**. Por exemplo, o contador de curtidas do card pode demorar milissegundos para refletir a curtida nova.
- Em um monólito modular, é possível aplicar a projeção na mesma transação como etapa de migração (ver [ADR-003](08-decisoes-arquiteturais.md)).

## 6.4 Evolução: eventos pelo broker

Com o RabbitMQ, cada evento deste catálogo vira uma mensagem com envelope fixo (`eventId`, `type`, `version`, `occurredAt`, `aggregateId`, `payload`) e chave de roteamento no formato `tutorial.published`. As filas e os padrões de interesse de cada consumidor estão em [camada de mensageria, seção 9.4](09-camada-de-mensageria.md). A consistência entre contextos continua **eventual**.
