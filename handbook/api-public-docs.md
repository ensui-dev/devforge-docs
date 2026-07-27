---
title: API reference: public documentation
type: API
internal: false
related: api-documents
documents: publishing
---

# API reference: public documentation

The published-documentation endpoints. These are the only ones that need **no
token** — that is the point of them.

## Reading published documentation

| Method | Path | Auth |
|---|---|---|
| `GET` | `/api/public/docs` | none |
| `GET` | `/api/public/docs/{handle}` | none |
| `GET` | `/api/public/docs/{handle}/{workspaceSlug}` | none |
| `GET` | `/api/public/docs/{handle}/{workspaceSlug}/{documentSlug}` | none |
| `GET` | `/api/public/instance` | none |

### `GET /api/public/docs`

Every workspace that has published, with how many pages each offers:

```json
[{
  "name": "DevForge Handbook", "slug": "devforge-handbook",
  "description": "How DevForge works, written in DevForge.",
  "pageCount": 23, "publishedAt": "2026-07-25T15:12:00Z"
}]
```

### `GET /api/public/docs/{handle}`

Everything one owner has published, like a profile page:

```json
{
  "handle": "acme",
  "workspaces": [{ "name": "Platform", "slug": "platform", "ownerHandle": "acme",
                   "publicPath": "/docs/acme/platform", "pageCount": 12,
                   "description": "…", "publishedAt": "…" }],
  "movedTo": null
}
```

`movedTo` carries a path when the segment is **not** a handle but does resolve to
exactly one published workspace — a link written before slugs were namespaced.
Clients redirect to it rather than showing a 404. When two owners share the slug it
stays null, because there is no single right answer.

### `GET /api/public/docs/{handle}/{workspaceSlug}`

One workspace's table of contents. Pages marked internal are absent.

```json
{
  "name": "DevForge Handbook", "slug": "devforge-handbook",
  "ownerHandle": "handbook", "description": "…",
  "entries": [{ "id": "…", "title": "Welcome to DevForge",
                "slug": "welcome", "documentType": "GENERAL" }]
}
```

### `GET /api/public/docs/{handle}/{workspaceSlug}/{documentSlug}`

One page, with its body and the reference edges between **public** pages:

```json
{
  "id": "…", "title": "The typed reference graph", "slug": "reference-graph",
  "content": "# The typed reference graph\n…",
  "documentType": "ARCHITECTURE",
  "references": [{ "referenceType": "DEPENDS_ON", "outgoing": false,
                   "relatedDocumentSlug": "tutorial-linking",
                   "relatedDocumentTitle": "Tutorial: linking documents together" }],
  "updatedAt": "…"
}
```

An unpublished workspace, or a page marked internal, returns **404** — the same
answer as something that does not exist, so neither can be probed.

### `GET /api/public/instance`

How this instance brands itself, whether it has been set up, and whether it
accepts registrations — everything a client needs before anyone signs in:

```json
{
  "configured": true,
  "name": "DevForge",
  "tagline": "Documentation and delivery, connected.",
  "logoMark": "⌁",
  "accentColor": null,
  "registrationMode": "OPEN",
  "allowedEmailDomains": [],
  "publicDocsEnabled": true,
  "handbookPath": "handbook/devforge-handbook"
}
```

Operational settings such as the instance's public address are deliberately not
included; those need an instance administrator and `GET /api/instance`.

## Controlling publication

These need a token, and `ADMIN` to change anything.

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces/{workspaceId}/publication` | `VIEWER` |
| `PUT` | `/api/workspaces/{workspaceId}/publication` | `ADMIN` |

`GET` describes the state, including what publishing would expose:

```json
{
  "published": true,
  "publishedAt": "2026-07-25T15:12:00Z",
  "publicPath": "/docs/handbook/devforge-handbook",
  "publicPages": 23,
  "internalPages": 0
}
```

`PUT` takes `{ "published": true }` or `{ "published": false }` and returns the same
shape. Publishing is idempotent: doing it twice keeps the original date.

Rejections: publishing with no public pages → `400`; changing it without `ADMIN` →
`403`; a workspace you are not in → `404`.

## Marking a page internal

Through the ordinary document endpoints — `internal` is a field on the document,
not a separate call:

```json
{ "title": "Scratch notes", "slug": "scratch-notes", "content": "…",
  "documentType": "GENERAL", "internal": true }
```

It defaults to `false`, so an existing client keeps working unchanged.
