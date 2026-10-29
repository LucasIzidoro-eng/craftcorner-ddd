# bootstrap/messaging

Implementação da camada de mensageria (RabbitMQ com padrão outbox). Ver [camada de mensageria](../../../docs/09-camada-de-mensageria.md) e o ADR-008.

| Arquivo previsto | Função |
|---|---|
| `OutboxEventPublisher.js` | Implementa a porta `EventPublisher`: grava o evento na tabela `outbox`, na mesma transação do agregado. |
| `OutboxRelay.js` | Lê os eventos pendentes da outbox, publica no RabbitMQ e os marca como enviados. |
| `RabbitMqConnection.js` | Conexão (amqplib), canal com confirmação de publicação, exchange `craftcorner.events`, filas, reenvio e filas de erro. |
| `EventSubscriber.js` | Consome as filas, controla `ack`/`nack` e despacha cada evento aos handlers dos contextos. |
| `IdempotencyGuard.js` | Registra os `eventId` já processados (`processed_events`) para ignorar mensagens repetidas. |
| `EventEnvelope.js` | Monta e lê o envelope JSON do evento (`eventId`, `type`, `version`, `occurredAt`, `aggregateId`, `payload`). |

Os contextos não importam nada desta pasta: eles só conhecem a porta `EventPublisher`, do `shared-kernel`.
