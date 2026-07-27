---
title: Use case: onboarding a new engineer
type: GENERAL
internal: false
depends_on: tutorial-boards, tutorial-linking-tasks
---

# Use case: onboarding a new engineer

The usual failure: a new starter is handed a wiki with 200 pages and no idea which
40 matter, then asks the same six questions in Slack.

## The setup

Create a workspace per team. Add three documents first:

1. **Start here** (`GENERAL`) — what the team owns, in a page.
2. **Local environment** (`RUNBOOK`) — getting the system running.
3. **Architecture overview** (`ARCHITECTURE`) — the shape of the thing.

Link them: *Local environment* `DEPENDS_ON` *Architecture overview*, so a starter
who hits something confusing in setup can see what explains it.

## The onboarding board

Create a board named **Onboarding**. Rename the columns to **Week 1**,
**Week 2**, **Ongoing**.

Add a task per milestone, and **link each task to the document that explains it**:

| Task | Linked document |
|---|---|
| Get the stack running locally | Local environment |
| Read the architecture overview | Architecture overview |
| Ship a one-line change to production | Release procedure |
| Take a turn on support rota | Incident runbook |

Now the board *is* the reading list, in order, with the material attached.

## Why this works

The starter never asks "what should I read?" — the board answers it. And when a
procedure changes, you update one document; every task pointing at it is current
immediately, and the reference graph shows you which those are.

## Keep it honest

Give the new starter `MEMBER` and ask them to fix what they find wrong on their
first pass. They are the only person who will ever read the onboarding docs with
fresh eyes.
