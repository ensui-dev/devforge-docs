---
title: Tutorial: running a delivery board
type: PROCEDURE
internal: false
implements: quickstart
---

# Tutorial: running a delivery board

## Creating a board

**Boards → New board.** Every board is seeded with **Backlog**, **In Progress**,
**Review**, and **Done**. Rename, reorder, or replace them.

## Columns

Hover a column header for its controls:

- **‹ ›** move the column left or right. Siblings renumber automatically.
- **⋯** open settings: rename, set a work-in-progress limit, or delete.

A board must keep at least one column, so the last one cannot be deleted.

### Work-in-progress limits

Set a limit and the column header shows `3/5`. It turns amber at the limit and red
above it.

The limit is checked when a task **arrives** — creating or moving one in. It is
not checked when you lower the limit, so an over-full column can always be
drained rather than being frozen.

## Tasks

**New task**, or **+ Add task** at the foot of a column. A task carries a title,
description, priority, an assignee, and any number of linked documents.

Only `HIGH` and `CRITICAL` priorities show a badge — `MEDIUM` is the default and
badging it would add noise to every card.

An assignee must be a member of the workspace. Assigning work to someone who
cannot open the project is rejected.

## Moving tasks

**Drag a card** between or within columns. A teal insertion line shows exactly
where it will land. The move applies instantly and reconciles with the server; if
the server refuses — a WIP limit, say — the card snaps back.

Prefer the keyboard? Open the task and change its **Column** field. Same result.

Positions always stay contiguous. Delete a task from the middle of a column and
the rest close the gap.

## Editing versus moving

Editing a task never changes where it sits. Column and position move through a
separate action, so a routine edit cannot silently reorder your board.
