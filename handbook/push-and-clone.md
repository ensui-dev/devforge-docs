---
title: Pushing and cloning
type: PROCEDURE
internal: false
related: self-hosting, sync-from-git
depends_on: tutorial-writing-documents
documents: roles-and-permissions
---

# Pushing and cloning

Every workspace has a git repository here. Clone it, write in your own editor, push,
and the pages follow. Edit a page in the interface and that becomes a commit in the
same repository, authored by whoever made it — so the two are one thing seen two
ways, not two copies to keep in step.

This is different from **Writing documentation in git**, which follows a repository
hosted somewhere else. Here, DevForge *is* the remote.

## Getting the address

**Workspace → Settings → Push and clone.** The remote URL is

```
https://this-instance/git/{owner-handle}/{workspace-slug}.git
```

Nothing has to be created first. The repository appears the moment you clone or push
to it, exactly as it does on any git host.

## Signing in

Git speaks HTTP Basic, so you need a token rather than an SSH key — **Account → Git
access → Issue a token**. Use it as the password; any username works, because the
token identifies its owner.

The secret is shown once. Only its digest is stored, so nothing can show it again;
issue a new one if you lose it. A token carries exactly the access its account has,
so revoking it, or the account, cuts off everything using it.

```bash
git clone https://this-instance/git/ada/handbook.git
# Username: anything
# Password: dfg_...
```

To let git remember it, use a credential helper — `git config credential.helper store`
writes it to a file in your home directory, and your operating system's keychain
helper is better if you have one.

| | Role needed |
|---|---|
| Clone, fetch, pull | **Viewer** |
| Push | **Member** |

There is no anonymous access. Published documentation is served at `/docs`; a
repository is not a second public surface, so a workspace nobody may read is
indistinguishable from one that does not exist.

## Pushing

Push to the default branch, `main`. Files are read exactly as they are on the sync
path: `.md` and `.markdown` only, the path below the documentation folder becomes the
slug, and front matter sets the title, type, and typed links.

The result comes back on the push itself:

```
$ git push devforge main
remote: DevForge: 2 created, 1 updated, 0 withdrawn, 4 unchanged
```

Which is the one moment you are looking, so a file that could not be applied says so
there rather than waiting to be discovered on a settings screen.

Other branches are ignored. A repository is a working space — drafts, feature
branches — and importing every one of them would make the workspace show whatever was
pushed last rather than what the documentation says.

A push that would withdraw every page is refused, for the same reason it is on the
sync path: a moved folder and an emptied one look identical, and the destructive
reading is not a guess worth making.

## Edits made here

Saving a page writes a commit:

```
$ git log --oneline -3
a1b2c3d Update runbooks/consumer-lag
9f8e7d6 Add docs/design
5c4b3a2 documentation
```

The author is whoever made the edit, so `git log` and `git blame` answer the same
question the page's **History** does. Creating, renaming and deleting a page all
show up the same way.

A page is rewritten where it already lives. If a file is called
`Getting Started.md`, editing that page changes *that* file rather than adding a
second one called `getting-started.md` saying the same thing.

### When both sides changed a page

Nothing is merged. The commit replaces one file on top of the current tip and touches
nothing else, so a push that changed other pages survives untouched. For the page
itself the edit wins — which is the right way round, because a push is applied to the
document immediately, so anyone editing in the interface is editing what the push
left behind.

If a push lands in the same instant, the write notices that the branch moved and
redoes itself on top of it rather than overwriting it.

### It cannot break a save

Committing happens after the edit is saved and answered. A full disk, a corrupt
repository, or git being switched off cannot make a page fail to save — the mirror
falls behind instead, and says so in the workspace's activity log.

## What this means for backups

Repositories live on the server's filesystem, not in PostgreSQL. `pg_dump` does not
capture them. A backup has to cover both the database and the git directory, and a
restore that only has the database will come back with the documents but without the
repositories they were pushed from.

## Using both directions at once

A workspace can follow an external repository *and* host its own, but they are two
different remotes and neither is copied into the other. If a workspace syncs from
GitHub, treat GitHub as the source of truth and use the hosted repository for reading
rather than writing — otherwise the next sync from GitHub overwrites what you pushed
here.
