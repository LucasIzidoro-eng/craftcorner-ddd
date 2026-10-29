# Tutorial Catalog / domain / value-objects

Objetos de valor: imutáveis, comparados pelo conteúdo, e que validam a si mesmos ao serem criados.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| Title.js | Obrigatório, trim antes de validar, até 100 caracteres. |
| Description.js | Até 500 caracteres. |
| CoverImage.js | Referência ao arquivo de imagem. |
| TutorialVideo.js | Referência ao arquivo de vídeo; imutável depois de publicado. |
| Visibility.js | Publicado ou Oculto. |
| CategoryName.js | Nome único e não vazio. |
