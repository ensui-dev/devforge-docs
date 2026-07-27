---
title: Troubleshooting
type: RUNBOOK
internal: false
related: api-errors
depends_on: running-locally
---

# Troubleshooting

## "Secure Connection Failed" / `SSL_ERROR_RX_RECORD_TOO_LONG`

Your browser tried HTTPS. Nothing here serves TLS.

Type **`http://localhost:3000`** with the prefix. If the browser has cached the
upgrade, use a private window, or clear the `localhost` entry under
`about:networking#hsts` (Firefox) or `chrome://net-internals/#hsts` (Chrome).

## Backend will not start: "password authentication failed"

Something else is already using port 5432 — often another project's PostgreSQL.
Check with `docker ps`, then either stop it or move DevForge:

```bash
DEVFORGE_DB_PORT=5433 docker compose up -d postgres
export DEVFORGE_DB_URL=jdbc:postgresql://localhost:5433/devforge
```

## Backend will not start: JWT secret rejected

```
Binding validation errors on devforge.jwt
  secret: must be at least 32 characters
```

Working as intended. Generate a real one: `openssl rand -base64 48`.

## Everything returns 401

The token expired — they last 12 hours and there is no refresh yet. Sign in again.
The app clears an expired session and redirects automatically.

## A save returns 409

Someone edited the same record while you had it open. Reload and reapply your
change; the platform refuses to overwrite their work silently.

## Cannot add a member

They need a DevForge account with that exact address first. There are no email
invitations yet, so ask them to register.

## Cannot move a task into a column

The column is at its work-in-progress limit. Either finish something already in
it, or raise the limit in the column's **⋯** settings.

## Tests fail with a Docker error

Backend integration tests start PostgreSQL through Testcontainers. Docker must be
running and your user must be able to reach the socket.
