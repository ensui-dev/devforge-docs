# DevForge handbook

The documentation for [DevForge](https://github.com/ensui-dev/devforge), kept as
markdown so it can be reviewed in a pull request like any other change.

This repository is a submodule of the main one, at `docs/`. It is also a DevForge
workspace: the same files are the pages served at
[devforge.ensui.dev/docs](https://devforge.ensui.dev/docs), and the two stay in step
in both directions.

## Layout

```
handbook/           the pages — one markdown file per page
```

The documentation folder is `handbook/` rather than the repository root, so this
README is not itself a page. A file's path below that folder is its slug, folders
included, so `handbook/welcome.md` is served at `/docs/{owner}/{workspace}/welcome`.

Renaming a file changes a page's address. Everything linking to it — front matter
here, links written in the interface, a bookmark someone kept — is pointing at the
old slug, so treat a rename as the breaking change it is.

## Front matter

Every page declares what it is, and what it points at:

```markdown
---
title: Writing documentation in git
type: PROCEDURE
internal: false
depends_on: tutorial-writing-documents
documents: document-types
---
```

`type` is one of `GENERAL`, `CODE`, `PROCEDURE`, `TECHNOLOGY`, `TECH_STACK`,
`ARCHITECTURE`, `API`, `RUNBOOK`, `DECISION`. `internal: true` withdraws a page from
the published site without deleting it.

The remaining keys are the typed reference graph — `related`, `depends_on`,
`implements`, `documents`, `supersedes` — naming target slugs, comma separated. Only
outgoing links are declared; the page you point at gains its backlink automatically.

A file that declares no relationship is not managing its links, and syncing leaves
whatever was made in the interface alone. Once it declares one, this file owns that
page's outgoing links entirely.

## Working on it

Clone it on its own, or as part of the main repository:

```bash
git clone --recurse-submodules https://github.com/ensui-dev/devforge.git
```

Edit the markdown, open a pull request, merge. Which is the point: the reference
graph gets reviewed alongside the change that made it true, rather than being
rearranged in a wiki afterwards.

## How it reaches an instance

An instance follows this repository from **Workspace → Settings → Sync from git**,
with the documentation folder set to `handbook`. A webhook applies every push.

A page edited in the interface is committed back to the repository that instance
hosts, not to this one — following a remote is one-way. If you edit here and there,
this repository wins on the next sync, and the version typed in the interface stays
in that page's history rather than being lost.

## Licence

MIT, the same as DevForge itself. See `LICENSE`.
