---
title: Document types
type: GENERAL
internal: false
---

# Document types

Every document has a type. It drives filtering, the tag shown on cards, and — more
importantly — it tells a reader what kind of thing they are about to read.

| Type | Use it for |
|---|---|
| `ARCHITECTURE` | System structure, boundaries, data flow |
| `DECISION` | An ADR: context, options considered, the choice, consequences |
| `API` | An interface contract: endpoints, payloads, error semantics |
| `CODE` | How a module, class, or algorithm works |
| `PROCEDURE` | A repeatable process: release steps, onboarding, review |
| `RUNBOOK` | Operating and recovering the system under pressure |
| `TECHNOLOGY` | One library or tool, and how this project uses it |
| `TECH_STACK` | The overall set of technologies, and why |
| `GENERAL` | Anything that does not fit above |

## Choosing well

The useful test: **what would a reader want from this page at 3am during an
incident?** If the answer is "steps I can follow", it is a `RUNBOOK`. If it is
"why is it built this way", it is `ARCHITECTURE` or `DECISION`.

Do not over-think it. Types are filters, not a taxonomy to be defended — and a
document's type can be changed at any time from the editor.

## Type is not visibility

A document's type says what kind of thing it is. Whether anyone outside the team
can read it is a separate setting — the *internal* flag, covered in **Publishing
your documentation**. A `RUNBOOK` is not automatically private, and a `GENERAL`
page is not automatically public.

## Filtering

The Documents screen has a filter chip per type. Filtering is disabled while a
search is active, because search already ranks across everything.
