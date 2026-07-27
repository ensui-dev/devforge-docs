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

## It tells you when two pages fall out of step

A graph that only tells you what *is* connected leaves the hard part to you. The
Connections panel also says which of those connections have drifted:

| On | It means |
|---|---|
| An outgoing link | The page you point at has changed since this one was last written, so what you wrote about it may no longer be true |
| A backlink | The reverse — you changed this page, and the page depending on it has not been touched since |

Both are the same fact seen from either end, so whichever page you happen to open,
the one that needs attention is marked.

Press the marker and you get exactly what changed on the other page **since your
page was last written** — not its whole history, which is a different question
with its own screen.

### It clears itself

There is no button to acknowledge anything. A page is out of step when the page it
points at has a newer revision than its own, so editing it — which is what you were
going to do anyway — is what brings the two back into step.

That has one consequence worth knowing: reading the change and deciding *nothing
needs to change here* does not clear the marker, because nothing recorded that you
looked. Adding a line saying why it is still correct does, and is worth more to the
next reader.

Saving a document without changing anything writes no revision, so it does not
count either. Only a real change moves a page forward.

## Using it well

Prefer `DEPENDS_ON` when you mean "this could invalidate that". It is the edge
you will traverse when planning a change — and the one whose marker will tell you
that something you relied on has moved.

Use `SUPERSEDES` instead of deleting a decision record. The history is usually
the valuable part.
