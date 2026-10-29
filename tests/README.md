# Estratégia de testes

Cada camada se testa de um jeito, e o domínio não precisa de banco nem de HTTP para ser testado.

| Camada | Tipo de teste | Dublês |
|---|---|---|
| `domain` | Unitário, rápido: invariantes dos agregados e dos objetos de valor (por exemplo: título só com espaços é recusado; ninguém segue a si mesmo). | Nenhum |
| `application` | Casos de uso com repositórios e portas **falsos** em memória (por exemplo: curtir duas vezes desfaz a curtida e publica `TutorialUnliked`). | Fakes das portas |
| `infrastructure` | Contrato dos repositórios contra um MySQL de teste (por exemplo: o índice único recusa a curtida repetida). | Banco de teste |
| `interfaces` | Ponta a ponta pela API (401, 403, 404, 200 e 201). | Banco de teste |
