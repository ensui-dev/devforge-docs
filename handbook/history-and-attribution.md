---
title: History and attribution
type: PROCEDURE
internal: false
related: api-documents
depends_on: tutorial-writing-documents
documents: roles-and-permissions
---

# History and attribution

Every change to a document is kept, and every change anywhere is attributed. Two
separate things, so they are worth separating.

## Document history

Open a document and press **History**. You get every revision it has had, newest
first, with who wrote each one and what has changed since.

Revision 1 is the document as created; every edit appends another. Picking one shows
a line-by-line diff against the live version — removals in red, additions in green,
each marked with `−` or `+` so the diff still reads without colour.

### Restoring never loses anything

Restoring revision 3 does not delete revisions 4 and 5. It writes a **new** revision
carrying revision 3's content, and records that it came from 3. So:

- the restore is itself visible in history
- you can undo a restore by restoring what you had before
- nothing anyone wrote is ever removed by someone else's restore

That is why the confirmation does not warn you that the action cannot be undone. It
can.

### Saving without changing nothing

Saving a document you have not edited adds no revision and no log entry. Git refuses
an empty commit for the same reason: an entry that records nothing makes the ones
that do harder to find. Renaming counts as a change even when the body is identical.

## The activity log

Every workspace has an **Activity** tab: what changed, who changed it, and when.
Visible to every member including viewers — it reveals nothing they cannot already
read, and knowing who last touched a page is part of reading it honestly.

Entries are never edited or deleted. Filter by kind if a busy workspace buries what
you are looking for.

An entry records only the fields that actually moved:

```
Ada Lovelace edited Event ingestion pipeline
  title: Design → Event ingestion pipeline
  length: 1841 → 2213
```

The actor and the target are stored as the names they had **at the time**. Rename an
account tomorrow and yesterday's entries still read correctly; delete it and they
still say who it was. A log that rewrote itself whenever the present changed would
not be a log.

Some entries have no actor at all. First-run setup happens before any account
exists, so it is attributed to *Setup* rather than to whoever was created by it.

## Instance-wide activity

Instance administrators get **Instance settings → Activity log**: everything on the
deployment, including events belonging to no workspace — setup, account creation, and
administration grants.

A workspace's entries survive its deletion, which is deliberate: with a cascade,
deleting a workspace would delete the evidence that it was deleted. They stop being
reachable through the workspace's own tab, because that workspace no longer exists,
and remain visible here.

## What is not kept

- **Nothing prunes either table.** Both stay small — a log entry is a few hundred
  bytes, and a document body is stored once per distinct content, so a restore or a
  reverted edit adds none — but there is no retention policy yet.
- **Deleting a document deletes its revisions.** The audit entry recording the
  deletion is kept.
- **Reads are not logged.** This records changes, not access.
