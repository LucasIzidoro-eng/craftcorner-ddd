# bootstrap/workers

Processos independentes que consomem eventos do RabbitMQ fora da requisição HTTP. Um worker não é um contexto novo: ele hospeda os handlers de um contexto em um processo separado. Ver [camada de mensageria](../../../docs/09-camada-de-mensageria.md).

| Arquivo previsto | Contexto dono dos handlers | O que faz |
|---|---|---|
| `notificationWorker.js` | Community | Consome `tutorial.liked`, `comment.added` e `user.followed` (fila `notifications.q`), grava notificações e dispara e-mails. |
| `mediaWorker.js` | Tutorial Catalog | Consome `tutorial.published` e `tutorial.cover-replaced` (fila `media.q`), gera a miniatura, valida o vídeo e publica `MediaProcessed`. |
