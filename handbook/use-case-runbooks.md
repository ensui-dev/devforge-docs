---
title: Use case: runbooks that stay current
type: RUNBOOK
internal: false
related: troubleshooting
depends_on: reference-graph
---

# Use case: runbooks that stay current

A runbook is read at 3am by someone stressed. It is also the document most likely
to be quietly wrong, because it describes a system that keeps changing.

## Write for the reader you will actually have

- Numbered steps, in order, with the command to run.
- State what "recovered" looks like, so they know when to stop.
- Put the diagnosis *before* the fix.
- No prose they have to parse under pressure.

## Link the runbook to the system it operates

Give every runbook a `DEPENDS_ON` edge to the architecture and technology pages
it assumes:

```
[Consumer lag runbook] --DEPENDS_ON--> [Kafka topic conventions]
                       --DEPENDS_ON--> [Event ingestion pipeline]
```

## The payoff

When someone changes topic partitioning, they open **Kafka topic conventions**,
and the *Referenced by* panel shows the consumer lag runbook. They know, before
merging, that a runbook needs updating.

Without the graph, that runbook stays wrong until the next incident finds it.

## Close the loop after an incident

Add a task to the board for each runbook correction the incident exposed, and link
the task to the runbook. The fix then gets tracked like any other work rather than
living in someone's memory.
