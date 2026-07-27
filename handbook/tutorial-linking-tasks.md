---
title: Tutorial: connecting tasks to documentation
type: PROCEDURE
internal: false
depends_on: reference-graph, tutorial-boards
---

# Tutorial: connecting tasks to documentation

The other half of the interconnection idea: work should point at knowledge instead
of restating it.

## The problem

A task description that repeats the spec goes stale the moment the spec changes.
Worse, the person doing the work reads the stale copy.

## Linking

Open any task. Under **Linked documents**, pick a document and click **Link**.

A task may cite as many documents as it needs — a design, an ADR, a runbook.
A document may be cited by as many tasks as need it.

Only documents in the same workspace are offered, and the platform rejects a
citation of a document from anywhere else.

## What you get

- The **card on the board** shows the linked documents as short traces, so you can
  see at a glance which work carries context and which does not.
- The task dialog links straight through to each document.
- Deleting a document cleans up its citations automatically, so a task never
  points at something that no longer exists.

## In practice

Write the task title as the *outcome*, and let the linked document carry the
*detail*:

> **Title:** Partition the orders topic
> **Linked:** Kafka topic conventions (`TECHNOLOGY`), Event ingestion pipeline
> (`ARCHITECTURE`)

Now when conventions change, the reference graph shows every task that cited them.
