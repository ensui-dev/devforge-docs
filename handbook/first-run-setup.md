---
title: First-run setup
type: PROCEDURE
internal: false
depends_on: self-hosting
---

# First-run setup

The screen a fresh instance shows before it will show anything else.

## It runs once

Setup refuses forever once it completes. That is a security property, not a
convenience: a deployment briefly reachable before its operator finishes must not
be claimable by whoever gets there second, and the endpoint must never be usable
to mint an administrator on a running instance.

There is no recovery path in the product if someone else completes it first.
Bring the instance up on a closed port, or finish setup immediately.

## The four steps

| Step | What it decides |
|---|---|
| **Identity** | Name, tagline, and public address |
| **Appearance** | A mark — a character or an uploaded image — and an accent colour |
| **Access** | Whether people may sign themselves up, and from which email domains |
| **Operator** | The first administrator |

Everything except the operator account can be changed afterwards, from
**Instance settings**.

## Identity

The name appears in the header, on the sign-in screen, and in the browser tab.
The public address is where the instance is reachable; it is used to build
absolute links and is never shown to visitors.

## Appearance

The accent replaces exactly one design token. Links, primary actions, and active
state follow it; spacing, contrast, and the signal colours used for priority and
warnings do not move. A rebrand is a colour, not a redesign.

A logo image is stored as a data URI in the settings row, so self-hosting needs no
file server. Keep it under 64KB.

## Access

Choose the registration mode. `RESTRICTED` needs at least one email domain — an
instance that restricts registration to nothing accepts nobody, and the server
refuses those settings outright.

## Operator

The account created here administers the instance. It owns the settings, and on a
closed instance it is the only way to add anyone else. The password is confirmed
before it is used, because a typo here would lock you out of your own deployment
with no way back in.

Once setup finishes you are signed in as that account, and `/setup` is gone.
