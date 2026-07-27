---
title: Publishing your documentation
type: PROCEDURE
internal: false
related: tutorial-writing-documents
depends_on: instance-settings, roles-and-permissions
implements: quickstart
---

# Publishing your documentation

Any workspace can serve its documentation as a public site inside DevForge. The
handbook you are reading is one — it is an ordinary workspace that happens to be
published.

## Publishing

**Settings → Public documentation → Publish documentation.** You need the `ADMIN`
role or higher.

Your pages then become readable at:

```
/docs/your-handle/your-workspace-slug
```

by anyone with the link, with no account.

The first segment is **your handle** — a URL-safe name your account gets when you
register, derived from your email address. It namespaces everything you own, so two
teams can each publish a workspace called `nokia` without clashing. Your handle is
shown beside your name on the workspaces screen. Only documentation is exposed — **boards,
tasks, and your team list stay private**.

## What gets published

Everything except pages marked internal. That is the useful default: a twenty-page
handbook should not need twenty separate decisions.

The consequence worth understanding: **a page you write later is public as soon as
you save it.** So DevForge never lets that state hide.

| Where | What you see |
|---|---|
| Navigation rail, every screen | A *Documentation is public* banner |
| Beside each document's type | A **Public** or **Internal** badge |
| The document editor | *Keep this page internal*, with the effect spelled out |
| Settings | Counts of public and held-back pages |
| The publish confirmation | The exact number of pages about to go live |

## Holding a page back

Open the page, click **Edit**, and tick **Keep this page internal**. It stays
private whether or not the workspace is published, and it disappears from the
public contents immediately.

Use it for scratch notes, drafts, anything with credentials in it, and anything
written for the team rather than for readers.

> An internal page cannot leak through the reference graph either. If a public page
> links to an internal one, that edge is simply absent from the public site — the
> internal page's title is never shown.

## Going private again

**Settings → Make private.** The public URL stops resolving at once. Nothing is
deleted, and republishing later keeps the original publication date.

## The directory

`/docs` lists every workspace on the instance that has published. Your own
documentation appears there as soon as you publish it, with its page count.

## Rules worth knowing

- Publishing needs at least one page that is not internal. Publishing an empty site
  reads as broken, so DevForge refuses it and says why.
- Publishing is `ADMIN`; **seeing** whether a workspace is published is `VIEWER`,
  because anyone writing pages here needs to know they are writing in public.
- Your workspace slug is the second segment of the public URL. Renaming it in
  Settings changes the address, so avoid it once people have the link.
- Slugs only have to be unique **among your own** workspaces. Another team taking
  the obvious name no longer blocks you.
- `/docs/your-handle` lists everything you have published, like a profile page.
