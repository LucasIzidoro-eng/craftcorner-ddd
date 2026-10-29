# Feed & Discovery / domain

Coração do contexto: regras de negócio puras. Não conhece banco, HTTP, Express nem Sequelize.

> Este contexto **não possui agregados nem regras de domínio próprias**: ele é apenas o lado de leitura (read model) da plataforma. Os dados são montados a partir de eventos publicados pelos outros contextos, por isso a camada `domain` fica vazia de propósito.
