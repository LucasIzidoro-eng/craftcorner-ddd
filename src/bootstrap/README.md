# Bootstrap (raiz de composição)

Único lugar que conhece **todas** as peças: aqui os adaptadores são ligados às portas (injeção de dependência manual). Nenhum contexto importa outro contexto diretamente.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| `container.js` | Cria os repositórios, adaptadores e casos de uso de cada contexto e os conecta. |
| `InProcessEventBus.js` | Barramento de eventos em memória: implementa `EventPublisher` e entrega cada evento aos handlers inscritos. Implementação inicial e usada nos testes. Em produção é substituído pela camada de mensageria (ver [doc 09](../../docs/09-camada-de-mensageria.md)), sem mudar o domínio. |
| `database.js` | Conexão com o MySQL (Sequelize). |
| `http/app.js` | Monta o Express, monta as rotas dos contextos e registra o tratador de erros (sempre o último). |
| `http/errorHandler.js` | Traduz erros de domínio em HTTP e apaga do disco os arquivos órfãos de requisições que falharam. |
| `server.js` | Sobe o servidor. |
| `messaging/OutboxEventPublisher.js` | Implementa `EventPublisher` gravando o evento na tabela `outbox`, na mesma transação do agregado. |
| `messaging/OutboxRelay.js` | Processo em segundo plano: lê eventos pendentes da outbox, publica no RabbitMQ e os marca como enviados. |
| `messaging/RabbitMqConnection.js` | Conexão (amqplib), canal com confirmação de publicação, exchange, filas, reenvio e filas de erro. |
| `messaging/EventSubscriber.js` | Consome as filas, controla `ack`/`nack` e despacha cada evento aos handlers dos contextos. |
| `messaging/IdempotencyGuard.js` | Registra os `eventId` já processados (`processed_events`) para ignorar mensagens repetidas. |
| `messaging/EventEnvelope.js` | Monta e lê o envelope JSON do evento. |
| `workers/notificationWorker.js` | Processo que hospeda os handlers de notificação (contexto Community). |
| `workers/mediaWorker.js` | Processo que hospeda os handlers de mídia (contexto Tutorial Catalog): miniatura e validação do vídeo. |
