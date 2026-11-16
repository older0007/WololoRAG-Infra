# WololoRAG-Infra — Shared Database Infrastructure (PostgreSQL + Qdrant)

Dedicated Docker infrastructure repository providing the shared relational and vector databases consumed by both microservices on `localhost`:

1. **[WololoRAG](https://github.com/older0007/WololoRAG)** — **Indexer Worker (Write Side)**: crawls Wololo.net, splits articles into token chunks, generates embeddings, and persists state + vectors.
2. **[WololoRAG-MCP](https://github.com/older0007/WololoRAG-MCP)** — **MCP Server (Read Side)**: queries Qdrant and PostgreSQL to answer questions from LLM clients (Antigravity, Claude, Gemini).

---

## Services in `docker-compose.yml`

| Service | Address / Port | Purpose |
| :--- | :--- | :--- |
| **PostgreSQL 16** | `localhost:5432` | Relational `articles` table tracking topics, sync statuses (`DISCOVERED`, `INDEXED`, `OUTDATED`, `FAILED`), `vector_ids`, and `error_str` |
| **Adminer** | `http://localhost:8081` | Lightweight browser SQL client for inspecting tables visually |
| **Qdrant** | `localhost:6333` | Vector database + visual **Web Dashboard** at `http://localhost:6333/dashboard` |

---

## Quick Start

```bash
# 1. (Optional) Copy .env.example to .env to customize ports or credentials
cp .env.example .env

# 2. Start PostgreSQL, Adminer, and Qdrant in the background
docker compose up -d

# 3. Verify container health
docker compose ps
```

---

## Connecting Services via `.env`

In both `WololoRAG` and `WololoRAG-MCP`, set the following in `.env`:

```env
DATABASE_URL=postgresql+psycopg://wololo:wololo_pass@localhost:5432/wololo_rag
QDRANT_MODE=server
QDRANT_URL=http://localhost:6333
```

---

## Accessing Adminer (SQL Web UI)

1. Open `http://localhost:8081`
2. **System:** `PostgreSQL`
3. **Server:** `postgres`
4. **Username:** `wololo`
5. **Password:** `wololo_pass`
6. **Database:** `wololo_rag`
