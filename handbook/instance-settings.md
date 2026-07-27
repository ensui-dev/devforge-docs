---
title: Instance settings
type: PROCEDURE
internal: false
related: publishing
depends_on: first-run-setup
documents: roles-and-permissions
---

# Instance settings

Everything the operator of a self-hosted instance controls, at
**Instance settings** in the workspace header. Only an instance administrator sees
the link, and the server refuses everyone else.

## Registration modes

| Mode | Who can create an account | Suits |
|---|---|---|
| `OPEN` | Anyone who can reach the deployment | A public instance |
| `RESTRICTED` | Anyone with an email at a listed domain | A company instance on a public address |
| `CLOSED` | Nobody — the operator creates every account | A private or single-team instance |

Domains match exactly; subdomains are not included. `acme.com` does not admit
`mail.acme.com`.

## Adding people to a closed instance

**Operators → Add operator** does two things: it appoints someone already on the
instance, and it creates an account outright. Creation bypasses the registration
mode entirely — that is what makes `CLOSED` a usable setting rather than a locked
door. Clear the administrator box to create an ordinary account.

## Keep a second operator

An instance whose only administrator loses their password cannot be reconfigured
by anything inside the product. The settings screen refuses to remove the last
administrator and says why. Appoint a second one early.

An operator can step down once a second one exists.

## Public documentation

The master switch. Switching it off refuses new publications **and** takes every
already-published site offline at once. Nothing is deleted: the workspaces stay
published in the database, and they reappear the moment it is switched back on.

The handbook path names which published workspace `/docs` opens by default, as
`handle/slug`. Leave it blank and `/docs` lists every published workspace on the
instance instead.

## What visitors can see

Branding is served from `GET /api/public/instance`, which needs no session — the
sign-in screen has to know the instance's name before anyone has signed in. That
response deliberately omits operational settings such as the public address, so
adding a setting later cannot accidentally publish it.
