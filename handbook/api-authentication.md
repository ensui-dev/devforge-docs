---
title: API reference: authentication
type: API
internal: false
documents: roles-and-permissions
---

# API reference: authentication

Base URL **`{{instance.url}}/api`** — this instance, as you are reading it. Running
the backend directly from source instead? That is `http://localhost:8080/api`.

All endpoints accept and return JSON.

Every route requires `Authorization: Bearer <token>` **except** register and
login. Interactive docs: `/swagger-ui.html`.

## `POST /api/auth/register`

Creates an account and returns a usable token. → `201`

```json
{ "email": "ada@example.com", "displayName": "Ada Lovelace", "password": "password123" }
```

Password must be at least 8 characters. Email is stored case-folded, so
`Ada@Example.com` and `ada@example.com` are the same account. Duplicate → `409`.

## `POST /api/auth/login`

Exchanges credentials for a token. → `200`

```json
{ "email": "ada@example.com", "password": "password123" }
```

Wrong password and unknown address return the **same** `401` message, so the API
cannot be used to discover which addresses are registered.

## Response shape

Both endpoints return:

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiJ9...",
  "tokenType": "Bearer",
  "expiresAt": "2026-07-26T02:00:00Z",
  "user": { "id": "…", "email": "ada@example.com",
            "displayName": "Ada Lovelace", "handle": "ada" }
}
```

`handle` is derived from the address at registration and suffixed if taken
(`ada`, `ada-2`, …). It namespaces the workspaces this account owns.

Tokens are valid for 12 hours. There is no refresh endpoint yet — when a token
expires, sign in again.

## `GET /api/auth/me`

Describes the authenticated user. → `200`

## `GET /api/users?q=`

Finds users to add to a workspace. Matches display name or email, minimum two
characters, capped at 20 results. → `200`

## History and activity

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/api/workspaces/{id}/documents/{docId}/revisions` | A document's revisions, newest first. Bodies omitted |
| `GET` | `/api/workspaces/{id}/documents/{docId}/revisions/{n}` | One revision in full |
| `POST` | `/api/workspaces/{id}/documents/{docId}/revisions/{n}/restore` | Restore it as a **new** revision |
| `GET` | `/api/workspaces/{id}/activity` | What changed in this workspace. `?action=` filters |
| `GET` | `/api/instance/activity` | Everything on the instance (instance admin) |

Reading either needs `VIEWER`; restoring needs `MEMBER`.

## Using a token

```bash
TOKEN=$(curl -s -X POST {{instance.url}}/api/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"email":"ada@example.com","password":"password123"}' \
  | python3 -c 'import sys,json;print(json.load(sys.stdin)["accessToken"])')

curl -s {{instance.url}}/api/workspaces -H "Authorization: Bearer $TOKEN"
```
