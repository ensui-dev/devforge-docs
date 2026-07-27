---
title: Tutorial: searching your knowledge base
type: PROCEDURE
internal: false
depends_on: document-types
---

# Tutorial: searching your knowledge base

## Using it

The search box sits at the top of the **Documents** screen. It searches **titles
and full body text** across the whole workspace, ranked by relevance, with title
matches weighted above body matches.

Results are paginated and update as you type. Clearing the box returns you to the
filtered list.

## What you can type

Search accepts ordinary web-search syntax:

| You type | It means |
|---|---|
| `retry policy` | Both words, anywhere |
| `"retry policy"` | That exact phrase |
| `retry -kafka` | Retry, excluding Kafka |
| `retry or backoff` | Either term |

Unbalanced quotes and stray operators are handled rather than rejected, so a
half-typed query never produces an error.

## How it stays correct

The search index is maintained by the database itself as part of each write, so it
can never drift from the content. Edit a document and the next search reflects it
immediately — there is no rebuild step and no background job to fall behind.

Searches are also scoped to the workspace you are in, so results never leak
between teams.

## Tips

- Search finds *body* text, so a term buried in a code block or table is
  findable.
- Filtering by type is disabled while searching, because ranking already spans
  every type.
- Cannot find a page? Try a word you would have written in the body rather than
  the title.
