# CraftCorner - Arquitetura DDD & Arquitetura Hexagonal 

Este repositório apresenta a reestruturação arquitetural do projeto **CraftCorner** utilizando os conceitos de **Domain-Driven Design (DDD)**, **Arquitetura Hexagonal (Ports & Adapters)** e os princípios **SOLID**.

---

## 1. Domínio de Aplicação

O **CraftCorner** é uma plataforma web voltada à comunidade de artesanato. A aplicação permite que usuários se cadastrem, explorem tutoriais interativos por categorias, acompanhem vídeos, marquem projetos concluídos e publiquem fotos de suas criações artesanais.

Além disso, a plataforma promove a interação entre artesãos por meio de curtidas, comentários, avaliações, favoritos e um sistema de seguidores com feeds personalizados (*Geral* e *Seguindo*). Na área administrativa, o sistema oferece gerenciamento de tutoriais, categorias e moderação de conteúdo, acompanhado de um dashboard de estatísticas da plataforma.

---

##  2. Bounded Contexts (Contextos Delimitados)

O sistema foi dividido em quatro subdomínios/contextos bem definidos:

| Contexto Delimitado | Responsabilidade | Entidades Principais |
| :--- | :--- | :--- |
| **Users (Usuários & Perfis)** | Gestão de cadastro, autenticação, perfil de artesão e rede de seguidores. | `User`, `Follow` |
| **Tutorial (Tutoriais & Categorias)** | Gestão e organização de tutoriais de artesanato, mídias e categorias. | `Tutorial`, `Category` |
| **Interaction (Interações & Comunidade)** | Feed, postagem de criações, comentários, curtidas, avaliações e progresso. | `Creation`, `Comment`, `Like`, `Rating`, `Favorite`, `ProjectProgress` |
| **Moderation (Administração & Moderação)** | Moderação de conteúdos/usuários, denúncias e métricas do dashboard. | `Moderation`, `ContentReport`, `DashboardStatistics` |

---

##  3. Estrutura de Pastas (Arquitetura)

```text
craftcorner-ddd/
├── README.md
└── src/
    ├── domain/               # Camada do Domínio (Regras de Negócio Puramente)
    │   ├── user/
    │   │   ├── entities/
    │   │   ├── value-objects/
    │   │   ├── repositories/ # Interfaces/Portas dos Repositórios
    │   │   └── services/
    │   ├── tutorial/
    │   │   ├── entities/
    │   │   ├── value-objects/
    │   │   ├── repositories/
    │   │   └── services/
    │   ├── interaction/
    │   │   ├── entities/
    │   │   ├── repositories/
    │   │   └── services/
    │   └── moderation/
    │       ├── entities/
    │       ├── repositories/
    │       └── services/
    ├── application/          # Camada de Aplicação (Casos de Uso)
    │   ├── user/
    │   │   └── use-cases/    # ex: RegisterUser, FollowUser
    │   ├── tutorial/
    │   │   └── use-cases/    # ex: CreateTutorial, SearchTutorials
    │   ├── interaction/
    │   │   └── use-cases/    # ex: PublishCreation, LikeTutorial
    │   └── moderation/
    │       └── use-cases/    # ex: ModerateContent
    ├── infrastructure/       # Camada de Infraestrutura (Tecnologias Externas)
    │   ├── database/         # Configurações do Sequelize / MySQL
    │   ├── repositories/     # Implementações concretas das interfaces do domínio
    │   ├── storage/          # Upload e manipulação de mídias (fotos/vídeos)
    │   └── security/         # Criptografia, tokens (JWT), etc.
    └── interfaces/           # Camada de Entrada / Adaptadores Web
        └── http/
            ├── controllers/  # Manipulação de requisições e respostas HTTP
            ├── routes/       # Definição de endpoints de API
            └── validators/   # Schemas de validação de dados
```

---

##  4. Arquitetura Hexagonal & Fluxo de Dados

A arquitetura garante que as **Regras de Negócio (Core/Domínio)** não dependam de tecnologias como **Express**, **Sequelize** ou **MySQL**.

       ADAPTADOR DE ENTRADA
          Express / HTTP
                |
                v
       PORTA DE ENTRADA
                |
                v
       CAMADA DE APLICAÇÃO
           Casos de uso
                |
                v
        DOMÍNIO DO NEGÓCIO
      Entidades e regras
                ^
                |
       PORTA DE SAÍDA
      ITutorialRepository
                ^
                |
       ADAPTADOR DE SAÍDA
    Sequelize / MySQL / Storage

### Exemplo de Fluxo (Criação de Tutorial):
1. **Entrada:** O controller Express recebe a requisição HTTP e repassa os dados validados ao Caso de Uso (`CreateTutorialUseCase`).
2. **Execução:** O Caso de Uso orquestra as regras do `Domain` (como verificação de requisitos e validação de categoria).
3. **Saída:** O Caso de Uso persiste o tutorial invocando a interface `ITutorialRepository`. A implementação em `Infrastructure` grava os dados no banco **MySQL** via **Sequelize**.

---

##  5. Princípios SOLID Aplicados



* **Single Responsibility Principle (SRP) — Princípio da Responsabilidade Única:** cada componente possui uma responsabilidade específica. Por exemplo, o CreateTutorialUseCase coordena a criação de tutoriais, enquanto o controller HTTP recebe requisições e retorna respostas.

* **Open/Closed Principle (OCP) — Princípio Aberto/Fechado:** os componentes devem permitir a extensão de comportamentos sem exigir modificações desnecessárias no código existente. Por exemplo, podemos criar uma nova implementação de armazenamento de mídias sem alterar o caso de uso que publica uma criação.

* **Liskov Substitution Principle (LSP) — Princípio da Substituição de Liskov:** implementações de uma mesma abstração devem poder substituir umas às outras sem comprometer o funcionamento esperado do sistema. Por exemplo, diferentes implementações de ITutorialRepository devem respeitar o mesmo contrato definido para salvar e consultar tutoriais.

* **Interface Segregation Principle (ISP) — Princípio da Segregação de Interfaces:** as interfaces devem ser específicas para as necessidades de seus consumidores, evitando que um componente dependa de operações que não utiliza. Por exemplo, separar as operações de consulta das operações de administração de tutoriais quando houver necessidades distintas.

* **Dependency Inversion Principle (DIP) — Princípio da Inversão de Dependência:** os componentes de alto nível devem depender de abstrações, e não diretamente de implementações concretas. No CraftCorner, os casos de uso dependem de interfaces de repositório, enquanto a infraestrutura fornece suas implementações usando tecnologias como Sequelize e MySQL.