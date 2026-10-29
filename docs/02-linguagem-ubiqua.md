# 2. Linguagem ubíqua

A mesma linguagem do negócio é usada nas conversas, na documentação e no código. A documentação está em português e o código em inglês, e a tabela abaixo é o dicionário entre os dois.

| Termo (negócio) | Definição | No código | Contexto dono |
|---|---|---|---|
| Tutorial | Conteúdo de ensino com título, descrição, imagem de capa e, opcionalmente, vídeo. | Tutorial | Tutorial Catalog |
| Categoria (tipo de artesanato) | Agrupamento dos tutoriais por técnica, como crochê ou amigurumi. | Category | Tutorial Catalog |
| Criador | Membro que publica tutoriais. Em outros contextos aparece apenas como `authorId`. | Author (authorId) | Tutorial Catalog |
| Capa | Imagem que representa o tutorial nos cards e como pôster do vídeo. | CoverImage | Tutorial Catalog |
| Publicado / Oculto | Visibilidade de um tutorial; oculto por moderação, sai dos feeds. | Visibility | Tutorial Catalog |
| Curtida | Marca de que uma pessoa gostou de um alvo. No máximo uma por pessoa e alvo. | Like | Engagement |
| Comentário | Texto de 1 a 500 caracteres, já aparado, com o autor. | Comment | Engagement |
| Avaliação | Nota de 1 a 5 estrelas dada por uma pessoa a um tutorial. | Rating | Engagement |
| Visualização | Uma abertura da tela de detalhe do tutorial (uma por carregamento). | View | Engagement |
| Estatísticas do tutorial | Curtidas, comentários, média de estrelas e visualizações, mantidas por eventos. | TutorialStats | Engagement |
| Projeto concluído | Registro de que uma pessoa terminou o projeto ensinado em um tutorial. | ProjectProgress | Learning & Creations |
| Criação | Foto de uma peça feita pela pessoa, com legenda, opcionalmente inspirada em um tutorial. | Creation | Learning & Creations |
| Seguir / Seguindo / Seguidor | Relação em que uma pessoa acompanha as publicações de um criador. | FollowRelation | Community |
| Perfil | Como a pessoa aparece publicamente: nome, biografia e foto. | Profile | Community |
| Feed Geral | Lista paginada de todo o conteúdo publicado, do mais novo ao mais antigo. | GeneralFeed | Feed & Discovery |
| Feed Seguindo | Lista apenas do conteúdo de quem a pessoa segue. | FollowingFeed | Feed & Discovery |
| Conta | Credenciais de acesso: e-mail, nome de usuário, senha (hash) e papel. | User | Identity & Access |
| Papel | Membro ou Administrador. Define o que a pessoa pode fazer. | Role | Identity & Access |
| Moderação | Decisão registrada de ocultar ou restaurar conteúdo, e de suspender ou reativar usuário, sempre com motivo. | ModerationAction | Administration & Moderation |
| Dashboard | Painel de estatísticas da plataforma para administradores. | Dashboard | Administration & Moderation |


## Um conceito, vários modelos

Em DDD, a mesma palavra pode significar coisas diferentes em cada contexto. Por isso cada contexto tem o **seu** modelo de "usuário", em vez de um único `User` gigante usado em todo lugar.

| Conceito do mundo real | Identity & Access | Community | Tutorial Catalog | Engagement |
|---|---|---|---|---|
| A pessoa | **Conta** (e-mail, senha, papel) | **Perfil** (nome, bio, foto) e **Seguidor** | **Criador** (`authorId`) | **Autor do comentário** (`authorId`) |
| O que importa | Credenciais e permissões | Como aparece e quem acompanha | Quem publicou | Quem curtiu ou comentou |

Do lado de fora de Identity & Access, a pessoa é só um `UserId`.

## Regras de nomeação
- Eventos no **passado** (`TutorialPublished`, nunca `PublishTutorial`).
- Comandos no **imperativo** (`PublishTutorial`).
- Consultas começam com `Get`, `List`, `Check` ou `Search`.
- Repositórios falam a linguagem do domínio (`findByUserAndTarget`), sem vazar SQL.
