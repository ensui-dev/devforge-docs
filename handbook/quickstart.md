---
title: Quickstart: your first workspace
type: PROCEDURE
internal: false
related: roles-and-permissions
depends_on: welcome
---

# Quickstart: your first workspace

About five minutes, start to finish.

## 1. Sign in

Open **{{instance.url}}** and create an account.

> Running a local stack from source instead? It is at `http://localhost:3000` —
> type the `http://`, because nothing local serves HTTPS and browsers otherwise
> try `https://` and fail with `SSL_ERROR_RX_RECORD_TOO_LONG`.

## 2. Create a workspace

Click **New workspace**. A workspace is one project or team: it owns its own
documents, boards, and members, and is invisible to everyone outside it.

The slug fills itself in from the name. It is used in links, so keep it short.

You become the workspace **owner** automatically.

## 3. Write your first document

Go to **Documents → New document**. Give it a title, pick a type, and write in
Markdown. The editor shows a live preview underneath as you type.

Start with something structural — an architecture overview or a decision record.
Later pages can then point at it.

## 4. Link two documents

Create a second document, then open it and click **Link document** in the
Connections panel. Choose a relationship and a target.

Now open the *first* document. The link is there too, phrased from its side — this
is the backlink, and you never had to create it.

## 5. Track the work

Go to **Boards → New board**. Every board starts with Backlog, In Progress,
Review, and Done; rename or reorder them however you like.

Add a task, open it, and use **Linked documents** to attach the page it depends
on. The card now shows that connection on the board itself.

## 6. Invite the team

**Team → Add member**, by email address. They need a DevForge account already.
Pick a role — see **Roles and permissions** for what each one can do.

## 7. Publish it, if you want to

**Settings → Public documentation** makes your pages readable by anyone with the
link. Boards and your team list stay private. See **Publishing your
documentation**.

## What next

- **Tutorial: linking documents together** goes deeper on the graph.
- **Use case: onboarding a new engineer** shows a realistic setup.
