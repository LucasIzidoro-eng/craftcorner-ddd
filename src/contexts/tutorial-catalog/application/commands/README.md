# Tutorial Catalog / application / commands

Casos de uso que alteram estado. Um arquivo por caso de uso.

## Arquivos previstos

| Arquivo | Responsabilidade |
|---|---|
| PublishTutorial.js | Recebe capa e vídeo, valida, salva (banco primeiro) e publica TutorialPublished. |
| ChangeTutorialDetails.js | Edição de título e descrição. |
| ReplaceTutorialCover.js | Troca a capa e apaga a antiga só depois de salvar. |
| DeleteTutorial.js | Exclui o tutorial, apaga os dois arquivos e publica TutorialDeleted. |
| CreateCategory.js | Cria categoria (Admin). |
| RenameCategory.js | Renomeia categoria (Admin). |
