---
title: Roles and permissions
type: PROCEDURE
internal: false
---

# Roles and permissions

A workspace is reachable only by its members. Roles are **ranked**, and every
capability is cumulative — a higher role can do everything a lower one can.

| Role | Can |
|---|---|
| `VIEWER` | Read documents and boards |
| `MEMBER` | Also create and edit documents, boards, and tasks |
| `ADMIN` | Also manage the team, delete boards, edit settings, publish documentation |
| `OWNER` | Also delete the workspace |

## Two rules that protect the team

**A workspace always keeps at least one owner.** The last owner cannot leave or
demote themselves. Promote someone else first.

**Nobody may exceed their own authority.** An admin cannot grant the owner role,
and cannot remove or re-role someone ranked above them. Otherwise `ADMIN` would be
a quiet path to `OWNER`.

## Publishing is an admin action

Making documentation public needs `ADMIN`. **Seeing** whether a workspace is
published needs only `VIEWER` — anyone writing pages in a published workspace has
to know that is what they are doing.

## Why a non-member sees "not found"

Ask for a workspace you are not in and you get **404**, not **403**. A 403 would
confirm the workspace exists, which lets an outsider enumerate other teams'
projects by guessing. The API refuses to distinguish "does not exist" from "not
yours".

This is also why the app hides controls you cannot use: the interface reflects
your role, and the backend enforces it independently.

## Adding someone

**Team → Add member**, by email. They must already have a DevForge account —
there are no email invitations yet.

Anyone can leave a workspace themselves; removing *another* person requires
`ADMIN`.
