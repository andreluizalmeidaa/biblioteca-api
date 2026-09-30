# API de Biblioteca — Node.js, Express, Sequelize e SQLite

Atividade prática de Desenvolvimento Web 2.

## Integrantes
- Integrante 1: André Luiz Farias Cavalcante de Almeida
- Integrante 2: Beatriz Dantas Cabral

## Como instalar
```bash
npm install
```

## Como executar
```bash
npm start
```
O servidor sobe em `http://localhost:3000` e o banco `database/biblioteca.sqlite` é criado automaticamente.

## Arquitetura
`Rota → Controller → Service → Repository → Model (Sequelize) → SQLite`

- **Controller:** recebe a requisição HTTP e devolve a resposta.
- **Service:** regras de negócio (ex.: verificar se o autor existe, validar filtros e paginação).
- **Repository:** consultas ao banco via Sequelize.

## Rotas disponíveis
| Método | Rota | Descrição |
|---|---|---|
| POST | /autores | Criar autor |
| GET | /autores | Listar autores |
| GET | /autores/:id | Buscar autor |
| PUT | /autores/:id | Atualizar autor |
| DELETE | /autores/:id | Excluir autor (bloqueado se tiver livros) |
| POST | /livros | Criar livro |
| GET | /livros | Listar livros (com autor e categorias, filtros e paginação) |
| GET | /livros/:id | Buscar livro |
| PUT | /livros/:id | Atualizar livro |
| DELETE | /livros/:id | Excluir livro |
| POST | /livros/:livroId/categorias/:categoriaId | Associar categoria a um livro |
| POST | /categorias | Criar categoria |
| GET | /categorias | Listar categorias |
| GET | /categorias/:id | Buscar categoria |
| PUT | /categorias/:id | Atualizar categoria |
| DELETE | /categorias/:id | Excluir categoria |

### Filtros e paginação em `GET /livros`
- `titulo` (busca parcial), `ano`, `disponivel` (true/false) — podem ser combinados
- `page` (padrão 1) e `limit` (padrão 10, máximo 100)

Exemplo: `GET /livros?titulo=dom&disponivel=true&page=1&limit=10`

```json
{
  "data": [{ "id": 1, "titulo": "Dom Casmurro", "Autor": { "id": 1, "nome": "Machado de Assis" } }],
  "pagination": { "page": 1, "limit": 10, "total": 1, "totalPages": 1 }
}
```

## Exemplos de requisições
```bash
curl -X POST localhost:3000/autores -H "Content-Type: application/json" \
  -d '{"nome":"Machado de Assis","email":"machado@email.com","nacionalidade":"Brasileiro"}'

curl -X POST localhost:3000/livros -H "Content-Type: application/json" \
  -d '{"titulo":"Dom Casmurro","isbn":"9780000000001","ano":1899,"autorId":1}'

curl -X POST localhost:3000/categorias -H "Content-Type: application/json" \
  -d '{"nome":"Romance","descricao":"Livros de romance"}'

curl -X POST localhost:3000/livros/1/categorias/1
```

## Códigos de resposta
200 OK · 201 criado · 204 excluído · 400 dados inválidos · 404 não encontrado · 409 conflito (valor único duplicado ou registro em uso)
