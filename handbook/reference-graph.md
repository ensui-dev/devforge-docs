---
title: The typed reference graph
type: ARCHITECTURE
internal: false
documents: welcome
---

# The typed reference graph

The core idea of DevForge. Worth understanding properly.

## The problem it solves

Documentation rots because nobody knows what a change affects. You edit the
authentication design, and three runbooks, two ADRs, and a service README quietly
become wrong. Nothing tells you, because a plain hyperlink carries no meaning and
points only one way.

## Edges have types

A reference in DevForge is a **directed, typed edge** between two documents in the
same workspace.

```
[Event ingestion pipeline] --DEPENDS_ON--> [Kafka topic conventions]
```

Read it forwards from the source. The five types:

- **`DEPENDS_ON`** — the source relies on the target. Change the target and the
  source may become wrong. This is the edge that answers "what breaks?"
- **`IMPLEMENTS`** — the source realises a design or spec described by the target.
- **`DOCUMENTS`** — the source describes the subject the target defines.
- **`SUPERSEDES`** — the source replaces the target, which is kept for history.
- **`RELATED`** — loose association, no dependency implied.

## Every edge is visible from both ends

This is the part that matters. Creating one edge gives you two views:

| Viewed from | Panel | Reads as |
|---|---|---|
| The source | *This document* | "Depends on Kafka topic conventions" |
| The target | *Referenced by* | "Required by Event ingestion pipeline" |

You create the link once, from the page that knows about the dependency. The
other page gets its backlink automatically. Nobody has to remember to update it.

## Rules the platform enforces

- A document cannot reference itself.
- Both ends must be in the same workspace — you cannot link across teams even
  with a valid document id.
- The same pair may hold several edges *of different types*, but not a duplicate
  of the same type.
- A link can only be removed from the document that declared it. Backlinks show
  no delete control, because the edge is not theirs to remove.
- Deleting a document removes every edge touching it.

## Using it well

Prefer `DEPENDS_ON` when you mean "this could invalidate that". It is the edge
you will traverse when planning a change.

Use `SUPERSEDES` instead of deleting a decision record. The history is usually
the valuable part.
