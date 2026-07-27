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

Words. Several words narrow the search rather than widening it — every term has to
match somewhere in the page.

You do not have to finish them. Typing `authenti` finds the authentication page,
because search matches prefixes as well as whole words. Nor do you have to spell
them correctly: `authentcation` finds it too.

| You type | It finds |
|---|---|
| `retry policy` | Pages containing both words |
| `deploym` | Deployment, before you finish typing it |
| `lag` | `consumer-lag-runbook`, matching inside the word |
| `deploymnet` | Deployment, despite the transposition |

Punctuation and operators are stripped rather than rejected, so a half-typed query
never produces an error. There is no special syntax to learn — quoting a phrase or
prefixing a minus sign has no effect beyond the words themselves.

## How it stays correct

The search index is maintained by the database itself as part of each write, so it
can never drift from the content. Edit a document and the next search reflects it
immediately — there is no rebuild step and no background job to fall behind.

Searches are also scoped to the workspace you are in, so results never leak
between teams.

## How results are ordered

Four ways to match, ranked by how confident each one is:

1. A whole word, stemmed — so "authenticate" finds "authentication".
2. A prefix of a word, exactly as written.
3. A fragment inside a word of the title.
4. A title that merely looks similar, which is what tolerates a typo.

Within that, titles outrank bodies. So the page you meant is usually first, and
the near-misses are still there underneath rather than missing.

## Tips

- Search finds *body* text, so a term buried in a code block or table is
  findable.
- Filtering by type is disabled while searching, because ranking already spans
  every type.
- Cannot find a page? Try fewer words. Every term has to match, so a long query
  is a narrow one.
