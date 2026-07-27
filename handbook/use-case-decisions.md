---
title: Use case: a decision record trail
type: DECISION
internal: false
depends_on: document-types, reference-graph
---

# Use case: a decision record trail

Architecture decision records are only useful if you can find the one that
explains the code in front of you — and know whether it is still current.

## Write each decision as a document

Use the `DECISION` type and a consistent shape:

```markdown
## Context
What forced a choice.

## Options
What was considered, and the trade-offs.

## Decision
What was chosen.

## Consequences
What this makes easy, and what it makes hard.
```

## Link decisions to what they affect

- The decision `DOCUMENTS` the component it governs.
- The implementation notes `IMPLEMENTS` the decision.

Now open any architecture page and its backlinks show which decisions produced it.

## Superseding, not deleting

When a decision is reversed, **do not delete the old one**. Write the new record
and link it `SUPERSEDES` → the old one.

The old page keeps a backlink reading *"Superseded by …"*, so anyone who arrives
at it from an old commit message or Slack thread immediately sees it is stale and
where to go instead.

That trail — *why we did this, why we stopped* — is usually more valuable than the
current state alone.

## Connect it to delivery

When a decision creates work, link the tasks to the decision record. Six months
later, "why is this like this?" is answerable from the board.
