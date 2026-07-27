---
title: API reference: errors and status codes
type: API
internal: false
related: api-authentication
---

# API reference: errors and status codes

Every failure returns the same shape:

```json
{
  "timestamp": "2026-07-25T14:59:38Z",
  "status": 409,
  "error": "Conflict",
  "message": "Workspace slug already exists: platform",
  "path": "/api/workspaces"
}
```

Validation failures add a `fieldErrors` map, so a client can put each message
beside the input that caused it:

```json
{
  "status": 400, "error": "Bad Request", "message": "Validation failed",
  "path": "/api/workspaces",
  "fieldErrors": { "slug": "must be lowercase alphanumeric with hyphens" }
}
```

## What each code means

| Code | Meaning |
|---|---|
| `400` | The request broke a rule — validation, or a business constraint such as the last owner leaving |
| `401` | Missing, malformed, or expired token; or wrong credentials at login |
| `403` | You are a member but your role is too low for this action |
| `404` | It does not exist — **or** it is not yours (see below) |
| `409` | Conflicts with existing data: duplicate slug, duplicate link, or a concurrent edit |
| `500` | A defect on the server. It is logged with a stack trace |

## Two deliberate choices

**404 hides other teams.** Anything inside a workspace you are not a member of
returns `404`, never `403`. A 403 would confirm the resource exists.

**500 means a server bug, not your mistake.** Unmapped exceptions are logged and
returned as `500`. They are deliberately *not* folded into `400`, because doing
that makes every internal failure look like a client error and hides real defects.

## Concurrency

Every record carries a version. If someone saved while you were editing, your
write returns `409` with a message telling you to reload — rather than silently
overwriting their work.
