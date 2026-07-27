---
title: Writing documentation in git
type: PROCEDURE
internal: false
related: history-and-attribution, push-and-clone
depends_on: tutorial-writing-documents
documents: document-types
---

# Writing documentation in git

Keep your documentation as markdown in a repository, push, and let this workspace
follow it. Review documentation in a pull request like any other change.

## Setting it up

**Workspace → Settings → Sync from git.** Give it a repository URL, a branch, and the
folder your markdown lives in. Press **Sync now** — you should not have to push a
commit to find out whether the settings are right.

Then wire the webhook so every push syncs:

1. Copy the **webhook URL** from the settings panel.
2. Press **Generate secret** and copy the value. It is shown once — it is stored
   encrypted, so nothing can show it again.
3. In your git host, add a webhook with that URL, content type
   `application/json`, and that secret.

Deliveries with no signature, or the wrong one, are refused. Until a secret exists
every delivery is refused, because a delivery that cannot be verified is not
accepted.

## How files become documents

The path below your documentation folder is the slug, folders and all:

| File | Slug |
|---|---|
| `docs/design.md` | `design` |
| `docs/runbooks/consumer-lag.md` | `runbooks/consumer-lag` |
| `docs/frontend/README.md` | `frontend/readme` |

So the tree you have in the repository is the tree you get here, and the page is
served at `/docs/{owner}/{workspace}/runbooks/consumer-lag`. Names are lowercased and
hyphenated a segment at a time, so `Runbooks/Consumer Lag.md` lands in the same place.

Two files can still collide if their names differ only in punctuation —
`Consumer Lag.md` and `consumer-lag.md` in one folder — which the sync reports rather
than letting one quietly overwrite the other.

Front matter is optional:

```markdown
---
title: Event ingestion pipeline
type: ARCHITECTURE
internal: false
---

# Event ingestion pipeline
```

Every key can be left out. Without a `title` the first `#` heading is used, and
failing that the filename — most documentation already opens with its own title, and
repeating it in front matter is duplication that goes stale. Without a `type`, the
default from the settings panel applies.

Only `.md` and `.markdown` files are read. Anything else in the repository is ignored.

## When a file is deleted

You choose, because the safe answer depends on the repository:

| Setting | What happens |
|---|---|
| **Mark it internal** (default) | Withdrawn from published documentation, history kept |
| **Delete it** | Removed, along with its revisions |
| **Leave it alone** | For a repository holding only part of the documentation |

The default is the recoverable one deliberately. Deleting is what git means by a
removed file, but a mistyped folder makes *every* file look removed at once.

Which is why: **a sync that finds no documentation at all refuses to withdraw
anything.** A repository that is genuinely empty is indistinguishable from a wrong
folder, so the destructive reading is rejected and the message tells you to check the
path.

## Editing here as well as in git

You can do both. The repository wins on sync, and nothing is lost — the version
someone typed here stays in the document's **History** as a revision, so you can see
exactly what happened and restore it.

Every revision applied from a repository is marked as synced rather than edited, so
history answers "why did this page change?" and not merely "when".

## Private repositories

Add a **personal access token** — not an SSH key. Syncing reads over HTTPS, by
downloading an archive of the branch, so there is no SSH connection for a key to
authenticate. If you have a deploy key set up for cloning, it will not work here.

Read access to the one repository is enough. Nothing is ever written back, so a
token with write scope grants more than this needs.

| Host | Where | Scope |
|---|---|---|
| GitHub | Settings → Developer settings → Personal access tokens → **Fine-grained** | This repository only, `Contents: Read-only` |
| GitLab | Project → Settings → Access tokens | Role `Reporter`, scope `read_repository` |
| Gitea / Forgejo | Settings → Applications → Generate token | `read:repository` |

A fine-grained GitHub token scoped to one repository is the narrowest of these, and
what to prefer if the choice is offered.

The token is encrypted before being stored, so a database dump does not hand over a
live credential, and it is never returned by the API — the settings screen only tells
you whether one is stored.

Rotating `DEVFORGE_JWT_SECRET` makes stored tokens unreadable. The sync then reports
that plainly and asks you to enter the token again.

## Authoring the reference graph

This is the part a wiki-in-git cannot do. Declare typed links in front matter, one
key per relationship, naming target slugs:

```markdown
---
title: Event ingestion pipeline
type: ARCHITECTURE
depends_on: kafka-topic-conventions, event-schema
implements: event-driven-design
---
```

The five relationships are `related`, `depends_on`, `implements`, `documents`, and
`supersedes`. Targets are comma separated, and may name a slug, a filename, or a path with folders
— `runbooks/consumer-lag`, `runbooks/Consumer Lag.md`, and
`runbooks/consumer-lag.md` all mean the same page.

Which means **the reference graph gets reviewed in a pull request**. Someone adding a
dependency between two documents shows up as a diff, with the rest of the change that
made it true.

### Order does not matter

Documents are all created first, and links resolved afterwards. A file may point at
one that appears later in the same repository — documentation is written as a graph,
not in dependency order.

### Backlinks still come free

Only outgoing links are declared. The page you point at gains its backlink
automatically, and never has to mention you — so a widely-referenced page does not
accumulate a list of everything that depends on it.

### Declaring links is opt-in, per file

A file that names no relationship is not managing its links, and syncing will not
touch them. So a repository of plain prose can sit alongside links made in the
interface without eating them.

Once a file *does* declare a relationship, the repository is managing that page's
outgoing links: they become exactly what the file says, and anything else is removed.

### A link to nothing is reported

A target that matches no document is listed as a problem on the settings screen. The
page itself still syncs — one bad link does not block a good document — but the typo
is visible immediately rather than being found by a reader months later.

## The other direction

This page is about following a repository someone else hosts. DevForge can also *be*
the repository — clone it, push to it, and have edits made in the interface come back
as commits. See **Pushing and cloning**.

## What this does not do yet

- **There is no history import.** A sync applies the current state of a ref; it does
  not replay a repository's commits into revisions.
- **Following a remote is one-way.** Edits made here are committed back to the
  repository DevForge hosts, never to a repository somewhere else.
