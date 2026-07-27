---
title: Tutorial: writing and editing documents
type: PROCEDURE
internal: false
depends_on: document-types
implements: quickstart
---

# Tutorial: writing and editing documents

## Creating

**Documents → New document.** You provide:

- **Title** — what it is called.
- **URL slug** — fills in from the title; used in links. Lowercase letters,
  numbers, and hyphens only.
- **Type** — see **Document types**.

The document is created empty. You write the body in the editor.

## Writing

Click **Edit** on a document. The body is Markdown, and a **live preview** renders
underneath as you type — so you can check a table or code fence before saving.

Supported: headings, lists, tables, blockquotes, fenced code blocks with language
hints, inline code, links, images, and horizontal rules.

````markdown
## Retry policy

Consumers retry with exponential backoff:

```java
Retry.backoff(3, Duration.ofMillis(200))
```

| Attempt | Delay |
|---|---|
| 1 | 200ms |
| 2 | 400ms |
````

Nothing is saved until you press **Save changes**. **Cancel** restores the
document as it was.

## Naming this instance in a page

A page that says "open http://localhost:3000" is wrong everywhere except the
machine it was written on — and DevForge is self-hosted, so most pages are read
somewhere else. Write the address as a variable instead:

| Variable | Resolves to |
|---|---|
| `{{instance.url}}` | the address this page is being served from |
| `{{instance.name}}` | this instance's configured name |

So `Open {{instance.url}}/app` renders as **{{instance.url}}/app** here, and as
their own address for anyone reading the same page on another instance. That is
what lets this handbook be seeded onto any deployment and stay correct.

Substitution happens when the page renders, not when it is saved, so the stored
content is portable. It is deliberately not a template language — there are no
conditionals or loops, an unrecognised `{{instance.something}}` is left visible so
a typo is obvious, and braces in code samples are untouched.

## A note on safety

Document bodies are rendered as HTML, and they are written by your teammates. Every
body is sanitised before it reaches the page, so a `<script>` tag in a document
cannot run. Ordinary links and images are untouched.

## Public or internal

If the workspace is published, the editor shows whether this page is public, and a
**Keep this page internal** control holds it back. An internal page stays private
whether or not the workspace is published.

Pages you create in a published workspace are public by default, so the badge
beside the document type always tells you which you are looking at. See
**Publishing your documentation**.

## Concurrent edits

If someone else saved while you were editing, your save is rejected with a
conflict rather than silently overwriting their work. Reload and reapply.

## Deleting

**Delete** on a document removes it, every reference edge touching it, and every
task citation of it. There is a confirmation step, and it cannot be undone.
