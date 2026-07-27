---
title: Welcome to DevForge
type: GENERAL
internal: false
---

# Welcome to DevForge

DevForge is where a software team keeps **what it knows** next to **what it is
doing**. Documentation lives beside the delivery board, and the two are linked, so
a task always points at the pages that explain it.

## Why another tool

A wiki tells you what the team wrote down. A board tells you what the team is
doing. Neither tells you *what breaks if this changes* — and that is the question
that actually costs teams time.

DevForge answers it with a **typed reference graph**. Documents do not merely
link to each other; every link carries a meaning:

| Relationship | Reads as |
|---|---|
| `DEPENDS_ON` | This page relies on that one |
| `IMPLEMENTS` | This realises that design |
| `DOCUMENTS` | This describes that subject |
| `SUPERSEDES` | This replaces that page |
| `RELATED` | Loose association |

Every link is visible from **both** ends. Open a page and you see what it relies
on *and* what relies on it. That second list is the one a wiki never gives you.

## Sharing what you write

A workspace can publish its documentation as a public site at
`/docs/your-handle/your-slug` — readable by anyone, no account needed, while your
boards and team list stay private. This handbook is one of those sites.

## It is open source

DevForge is free software under the MIT licence, at
<https://github.com/ensui-dev/devforge>. This instance is one deployment of it,
not a service you are renting — no paid tier, no telemetry, and self-hosting is
the intended way to use it. A fresh deployment configures itself through a
first-run setup screen: name, mark, accent, who may sign up, and the account that
will administer it.

See **DevForge is open source**, then **Self-hosting DevForge**.

## Where to go next

- New here? Read [Quickstart](#) — it takes about five minutes.
- Want the concept first? Read **The typed reference graph**.
- Wiring up a client? Start at **API reference: authentication**.

> This handbook is itself a DevForge workspace. Every page you are reading was
> created through the API, and the **Connections** panel on the right of each
> document is real. Click through it.
