---
title: DevForge is open source
type: GENERAL
internal: false
documents: welcome
---

# DevForge is open source

DevForge is free software under the **MIT licence**. The source is at
<https://github.com/ensui-dev/devforge>, and what runs on this instance is what is
in that repository.

## What that means here

You are reading this on one deployment. It is not a hosted service anybody is
renting to you — it is a copy of an open-source project, running on someone's
server. You can run your own, and nothing is withheld from it.

| | |
|---|---|
| **Licence** | MIT — use it commercially, fork it, relicense your changes |
| **No paid tier** | Nothing is gated behind a licence key or an edition |
| **No telemetry** | No analytics, no phone-home, no CDN. The application makes no outbound request of any kind |
| **Your data** | One PostgreSQL database you control, plus the git repositories DevForge hosts |

The no-telemetry claim is checkable, which is the point of shipping the source:
the frontend loads no external asset and declares no analytics dependency, and the
backend has no HTTP client at all.

## Run your own copy

```bash
git clone https://github.com/ensui-dev/devforge.git
cd devforge
cp .env.example .env
openssl rand -base64 48        # paste as DEVFORGE_JWT_SECRET
docker compose --profile full up -d
```

First boot opens a setup wizard. See **Self-hosting DevForge** for what it asks
and why, and **First-run setup** for the one step that cannot be repeated.

## Contributing

Issues and pull requests are welcome at
<https://github.com/ensui-dev/devforge/issues>.

Two things are worth knowing before you send a change. The module boundaries are
enforced by ArchUnit, so importing another module's internals fails the build
rather than review. And tests here are expected to fail when the thing they cover
breaks — before trusting a new one, break the code and watch it go red. Several
tests carry a comment naming the specific bug they were written against; those
comments are the point.

`CONTRIBUTING.md` in the repository has the rest.

## Reporting a security problem

Privately, through GitHub security advisories rather than a public issue. The
repository's `SECURITY.md` also lists what an operator has to get right — changing
the token signing key, finishing setup promptly, and keeping more than one
administrator.
