---
title: Tutorial: linking documents together
type: PROCEDURE
internal: false
depends_on: reference-graph
implements: quickstart
---

# Tutorial: linking documents together

This is the feature worth learning properly. See **The typed reference graph** for
the concept; this page is the mechanics.

## Creating a link

1. Open the document that *has* the dependency — the one that would become wrong.
2. In the **Connections** panel, click **Link document**.
3. Choose the **relationship**. Read it forwards: *this document → the one you
   pick*.
4. Filter and select the target. Documents you have already linked in this
   direction are not offered again.
5. **Add link**.

## Reading the panel

Connections splits into two groups:

- **This document** — edges you declared. Each has a remove control.
- **Referenced by** — backlinks. Phrased from your side, with no remove control,
  because the edge belongs to the other page.

So `A DEPENDS_ON B` appears on A as *"Depends on B"* and on B as *"Required by A"*.

## Worked example

Say you are changing how tokens are issued.

1. Open **Auth flow**, link `DEPENDS_ON` → **Token conventions**.
2. Open **Login runbook**, link `DEPENDS_ON` → **Auth flow**.
3. Now open **Token conventions**. Its *Referenced by* panel shows **Auth flow**.
   Open that, and its backlinks show **Login runbook**.

You have traced the blast radius of the change in two clicks, without searching.

## Removing a link

Use the **×** beside an edge under *This document*. If you want to remove a
backlink, open the document that declared it — the platform will not let you
delete it from the far end.
