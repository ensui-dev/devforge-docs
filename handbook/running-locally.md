---
title: Running DevForge locally
type: RUNBOOK
internal: false
depends_on: tech-stack
---

# Running DevForge locally

## Everything in containers

```bash
cp .env.example .env
openssl rand -base64 48        # paste as DEVFORGE_JWT_SECRET
docker compose --profile full up --build
```

App at **http://localhost:3000**. Compose reads `.env` automatically.

## For development

Database only, with the backend and frontend on your machine so both hot-reload:

```bash
docker compose up -d postgres

cd backend && ./mvnw spring-boot:run      # :8080
cd frontend && npm install && npm run dev # :5173
```

The Vite dev server proxies `/api` to the backend, so there is no cross-origin
traffic and CORS stays off.

> `./mvnw` does **not** read `.env` — only Compose does. Export the values first:
> `export $(grep -v '^#' .env | xargs)`

## Useful endpoints

| URL | What |
|---|---|
| `http://localhost:8080/swagger-ui.html` | Interactive API docs |
| `http://localhost:8080/v3/api-docs` | OpenAPI JSON |
| `http://localhost:8080/actuator/health` | Health, with liveness and readiness |

## Tests

```bash
cd backend && ./mvnw test     # needs Docker for Testcontainers
cd frontend && npm test && npm run lint && npm run build
```

## Configuration

| Variable | Default | Notes |
|---|---|---|
| `DEVFORGE_JWT_SECRET` | dev placeholder | **Change it.** Minimum 32 characters; the app refuses to start below that |
| `DEVFORGE_DB_URL` | `jdbc:postgresql://localhost:5432/devforge` | |
| `DEVFORGE_DB_USERNAME` / `_PASSWORD` | `devforge` | |
| `DEVFORGE_DB_PORT` | `5432` | Host port for the Compose database |
| `DEVFORGE_CORS_ORIGINS` | empty | Only needed if the client is on another origin |
| `DEVFORGE_HANDBOOK_SLUG` | `devforge-handbook` | Which published workspace `/docs` opens by default. Blank lists them all. Does **not** publish anything — that is per workspace |
| `PORT` | `8080` | |

Rotating the JWT secret invalidates every issued token, so everyone signs in
again.
