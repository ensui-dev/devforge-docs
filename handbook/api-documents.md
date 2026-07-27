---
title: API reference: documents and references
type: API
internal: false
related: api-errors
depends_on: api-authentication
documents: reference-graph
---

# API reference: documents and references

## Documents

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces/{id}/documents` | `VIEWER` |
| `GET` | `/api/workspaces/{id}/documents/search` | `VIEWER` |
| `GET` | `/api/workspaces/{id}/documents/{documentId}` | `VIEWER` |
| `GET` | `/api/workspaces/{id}/documents/by-slug/{slug}` | `VIEWER` |
| `POST` | `/api/workspaces/{id}/documents` | `MEMBER` |
| `PUT` | `/api/workspaces/{id}/documents/{documentId}` | `MEMBER` |
| `DELETE` | `/api/workspaces/{id}/documents/{documentId}` | `MEMBER` |

### Listing

`GET …/documents?documentType=RUNBOOK&page=0&size=25`

`size` is capped at 100. Listings return an **excerpt**, not the full body, so
payload size does not grow with document length:

```json
{
  "content": [{ "id": "…", "title": "…", "slug": "…", "excerpt": "First 200 characters…",
                "documentType": "RUNBOOK", "createdAt": "…", "updatedAt": "…" }],
  "page": 0, "size": 25, "totalElements": 42, "totalPages": 2, "last": false
}
```

Fetch a single document by id or slug to get its `content`.

### Search

`GET …/documents/search?q=retry+policy&page=0&size=25`

Same page shape. Ranked, with titles weighted above bodies. Accepts phrases in
quotes, `-` to exclude, and `or`.

### Creating and updating

```json
{ "title": "Retry policy", "slug": "retry-policy",
  "content": "# Retry policy\n\n…", "documentType": "PROCEDURE" }
```

`internal` is optional and defaults to `false`. Set it to `true` to hold the page
back from the public site — see **API reference: public documentation**.

Slug is unique **per workspace**, so two teams may both have `overview`.
Duplicate within a workspace → `409`.

`PUT` replaces all four fields. A concurrent edit loses with `409`.

## References

| Method | Path | Role |
|---|---|---|
| `GET` | `/api/workspaces/{id}/documents/{documentId}/references` | `VIEWER` |
| `POST` | `/api/workspaces/{id}/documents/{documentId}/references` | `MEMBER` |
| `DELETE` | `/api/workspaces/{id}/documents/{documentId}/references/{referenceId}` | `MEMBER` |

Create with:

```json
{ "targetDocumentId": "…", "referenceType": "DEPENDS_ON" }
```

`GET` returns outgoing edges **and** backlinks together, each flagged:

```json
[{
  "id": "…", "referenceType": "DEPENDS_ON", "outgoing": true,
  "relatedDocumentId": "…", "relatedDocumentTitle": "Kafka topic conventions",
  "relatedDocumentSlug": "kafka-conventions", "relatedDocumentType": "TECHNOLOGY",
  "createdAt": "…"
}]
```

`outgoing: false` means this document is the *target* — a backlink.

Rejections: self-reference → `400`; duplicate of the same type → `409`; target in
another workspace → `404`; deleting from the target side → `404`.
