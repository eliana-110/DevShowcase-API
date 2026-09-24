# DevShowcase API

API REST em Node.js e Express com persistência relacional em SQLite.

## Requisitos

- Node.js 22.13 ou superior
- npm

## Executar

```bash
npm install
npm start
```

A API fica em `http://localhost:3000`. O banco é criado automaticamente em `data/devshowcase.sqlite`. Para mudar a porta ou o caminho do banco, defina `PORT` ou `DATABASE_PATH` no ambiente (veja `.env.example`). O arquivo `.env` não é carregado automaticamente.

```bash
npm test
```

## Modelo relacional

- `profiles` 1:N `projects`, por `projects.profile_id`.
- `projects` N:N `technologies`, por `project_technologies`.
- `projects` 1:N `feedback`, por `feedback.project_id`.
- Chaves estrangeiras ficam ativas; registros dependentes são excluídos em cascata.

Os endpoints de cadastro de feedback não fazem parte do escopo solicitado. A estrutura já permite persistir e consultar opiniões relacionadas aos projetos; a listagem de projetos inclui `feedback`.

## Endpoints

| Método | Caminho | Corpo JSON | Resposta |
| --- | --- | --- | --- |
| POST | `/api/profiles` | `name`, `email`; opcionais `bio`, `avatarUrl` | 201, perfil |
| GET | `/api/profiles/:id` | — | 200, perfil |
| POST | `/api/technologies` | `name` | 201, tecnologia |
| GET | `/api/technologies` | — | 200, lista |
| POST | `/api/projects` | `profileId`, `title`; opcionais `description`, `repositoryUrl`, `demoUrl`, `technologyIds` | 201, projeto |
| GET | `/api/projects` | — | 200, lista com tecnologias e feedback |

Campos obrigatórios não aceitam texto vazio. URLs opcionais devem usar HTTP ou HTTPS. IDs devem ser inteiros positivos. Erros retornam `{ "error": "mensagem" }` com status 400 (validação), 404 (referência ausente) ou 409 (nome ou email duplicado).

## Testar no Postman

Importe `postman/DevShowcase API.postman_collection.json`, execute `npm start` e rode as requisições na ordem apresentada. A coleção salva automaticamente os IDs do perfil e da tecnologia criados. Abra o **Postman Console** (`View > Show Postman Console`) para ver cada requisição e sua resposta; os scripts da coleção também registram o status e o JSON recebido. A variável `baseUrl` pode ser alterada se a porta for diferente de 3000.
