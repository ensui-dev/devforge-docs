---
title: API reference: workspaces and members
type: API
internal: false
related: publishing
depends_on: api-authentication
documents: roles-and-permissions
---

# API reference: workspaces and members

## Workspaces

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces` | — |
| `POST` | `/api/workspaces` | — |
| `GET` | `/api/workspaces/{workspaceId}` | `VIEWER` |
| `PUT` | `/api/workspaces/{workspaceId}` | `ADMIN` |
| `DELETE` | `/api/workspaces/{workspaceId}` | `OWNER` |

`GET /api/workspaces` returns only workspaces you belong to, each with your role:

```json
[{
  "id": "…", "name": "Platform", "description": "Core services",
  "slug": "platform", "callerRole": "OWNER",
  "createdAt": "…", "updatedAt": "…"
}]
```

Create and update take:

```json
{ "name": "Platform", "description": "Core services", "slug": "platform" }
```

Slug must match `^[a-z0-9]+(?:-[a-z0-9]+)*$` and is unique **per owner**, not across
the instance — so another team's `platform` is no obstacle. A slug you already used
→ `409`. The creator is enrolled as `OWNER` automatically, and their handle
namespaces the workspace's public address.

`DELETE` removes every document, board, and task in the workspace. → `204`

## Publication

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces/{workspaceId}/publication` | `VIEWER` |
| `PUT` | `/api/workspaces/{workspaceId}/publication` | `ADMIN` |

Described in full under **API reference: public documentation**.

## Members

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces/{workspaceId}/members` | `VIEWER` |
| `POST` | `/api/workspaces/{workspaceId}/members` | `ADMIN` |
| `PUT` | `/api/workspaces/{workspaceId}/members/{memberUserId}` | `ADMIN` |
| `DELETE` | `/api/workspaces/{workspaceId}/members/{memberUserId}` | `ADMIN`, or yourself |

Add by email — the account must already exist, or `404`:

```json
{ "email": "grace@example.com", "role": "MEMBER" }
```

Change a role with `{ "role": "ADMIN" }`.

Rejections worth knowing:

- Granting a role above your own → `403`
- Acting on a member ranked above you → `403`
- Removing or demoting the last owner → `400`
- Adding someone already a member → `409`
