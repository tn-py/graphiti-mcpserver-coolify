# Graphiti MCP Server (Coolify Fork)

This fork of the original [Graphiti MCP Server](https://github.com/getzep/graphiti) is tailored for **containerized deployments via Coolify, Docker Compose, or similar platforms**.

Graphiti is a framework for building and querying temporally-aware knowledge graphs, specifically tailored for AI agents operating in dynamic environments. Unlike traditional retrieval-augmented generation (RAG) methods, Graphiti continuously integrates user interactions, structured and unstructured enterprise data, and external information into a coherent, queryable graph.

This fork streamlines Graphiti's MCP server into a Docker-first setup optimized for deployment orchestration, including Neo4j integration and SSE-based MCP transport.

---

## ✨ Highlights of This Fork

* Dockerfile tailored for `uv`-based Python builds
* Neo4j + Graphiti containerized setup via Docker Compose
* Preconfigured environment variable support with `.env`
* Out-of-the-box compatibility with Coolify
* Healthcheck-based service orchestration

---

## Quick Deployment with Docker Compose (Recommended)

### Prerequisites

* Docker + Docker Compose
* OpenAI API key

### 1. Clone This Repo

```bash
git clone https://github.com/tn-py/graphiti-mcpserver-coolify.git
cd graphiti-mcpserver-coolify
```

### 2. Create a `.env` File

```bash
cp .env.example .env
```

Set your OpenAI key and model preferences inside `.env`:

```env
OPENAI_API_KEY=your_openai_key
MODEL_NAME=gpt-4.1-mini
```

### 3. Run the Full Stack

```bash
docker compose up --build
```

This will:

* Launch Neo4j (v5.26) with dev memory presets
* Launch Graphiti MCP server
* Bind Graphiti to port `8000` (mapped to host port `3010`)

Access the MCP SSE endpoint:

```
http://localhost:3010/sse
```

---

## Directory Overview

```
.
├── Dockerfile                # Graphiti MCP build
├── docker-compose.yml       # Multi-service stack (Neo4j + Graphiti)
├── .env.example             # Env config template
├── graphiti_mcp_server.py   # Entry point
└── pyproject.toml           # Python deps
```

---

## Coolify Setup Notes

To deploy on Coolify:

1. Connect your GitHub repo containing this fork
2. Ensure `docker-compose.yml` is in the root
3. Set your environment variables via Coolify UI or `.env`
4. Deploy the app

Coolify will:

* Build the Graphiti service using the `Dockerfile`
* Launch Neo4j in a linked container
* Expose Graphiti on a public port (e.g., `3010`)

---

## MCP Client Integration (SSE)

Configure your MCP-compatible client like Cursor or Claude to point to:

```json
{
  "mcpServers": {
    "graphiti-memory": {
      "transport": "sse",
      "url": "http://localhost:3010/sse"
    }
  }
}
```

---

## Original Graphiti MCP Features

All upstream functionality remains intact:

* `add_episode`
* `search_nodes`, `search_facts`
* `get_episodes`, `delete_episode`
* `clear_graph`, `get_status`

---

## Requirements

* Docker Engine
* Python 3.10+ (for local development)
* Neo4j 5.26+
* OpenAI API key

---

## License

This fork retains the original license from the [Graphiti project](https://github.com/getzep/graphiti).

---

## Contributors

Forked from the excellent work by the team at [Zep](https://github.com/getzep).