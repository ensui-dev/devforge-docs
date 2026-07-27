---
title: API reference: boards and tasks
type: API
internal: false
depends_on: api-authentication
---

# API reference: boards and tasks

## Boards and columns

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces/{id}/boards` | `VIEWER` |
| `GET` | `/api/workspaces/{id}/boards/{boardId}` | `VIEWER` |
| `POST` | `/api/workspaces/{id}/boards` | `MEMBER` |
| `PUT` | `/api/workspaces/{id}/boards/{boardId}` | `MEMBER` |
| `DELETE` | `/api/workspaces/{id}/boards/{boardId}` | `ADMIN` |
| `POST` | `/api/workspaces/{id}/boards/{boardId}/columns` | `MEMBER` |
| `PUT` | `…/columns/{columnId}` | `MEMBER` |
| `PATCH` | `…/columns/{columnId}/position` | `MEMBER` |
| `DELETE` | `…/columns/{columnId}` | `MEMBER` |

`GET /boards` returns summaries with `columnCount` and `taskCount` rather than
nested content. `GET /boards/{id}` returns the full board: columns in order, each
with its tasks in order.

Creating a board takes `{ "name": "Delivery" }` and seeds four default columns.

Column create/update takes `{ "name": "In Progress", "wipLimit": 3 }`. Omit or
null `wipLimit` for unlimited; it must be at least 1.

**Every column mutation returns the whole board**, because reordering renumbers
siblings and a partial response would leave the client with stale positions.

A board must keep one column: deleting the last → `400`.

## Tasks

| Method | Path | Role |
|---|---|---|
| `POST` | `…/boards/{boardId}/tasks` | `MEMBER` |
| `PUT` | `…/tasks/{taskId}` | `MEMBER` |
| `PATCH` | `…/tasks/{taskId}/position` | `MEMBER` |
| `DELETE` | `…/tasks/{taskId}` | `MEMBER` |
| `POST` | `…/tasks/{taskId}/documents` | `MEMBER` |
| `DELETE` | `…/tasks/{taskId}/documents/{documentId}` | `MEMBER` |

Create:

```json
{ "title": "Partition the orders topic", "description": "…",
  "columnId": "…", "priority": "HIGH",
  "assigneeId": "…", "linkedDocumentIds": ["…"] }
```

Only `title` and `columnId` are required. Priority defaults to `MEDIUM`. The task
is appended to the end of the column.

`PUT` edits title, description, priority, and assignee **only** — it never moves
the task.

### Moving

```
PATCH …/tasks/{taskId}/position
{ "columnId": "…", "position": 0 }
```

`position` is a zero-based index; values past the end place the task last rather
than erroring, so a drag past the bottom behaves as intended. Both affected
columns are renumbered so positions stay contiguous.

Rejections: assignee not a member → `400`; column on another board → `404`;
destination at its WIP limit → `400`.

### Document citations

`POST …/tasks/{taskId}/documents` with `{ "documentId": "…" }`. Duplicate → `409`;
document from another workspace → `404`. Both endpoints return the updated task.
