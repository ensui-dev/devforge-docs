---
title: Self-hosting DevForge
type: PROCEDURE
internal: false
related: running-locally
depends_on: open-source, tech-stack
---

# Self-hosting DevForge

DevForge is open source under the MIT licence and built to be run by whoever uses
it. Nothing that distinguishes one deployment from another is baked into the
build — the same image serves a public instance, a company's private one, and a
single-person notebook. The difference is a row in the database.

## Bring it up

```bash
git clone https://github.com/ensui-dev/devforge.git
cd devforge
cp .env.example .env
openssl rand -base64 48        # paste as DEVFORGE_JWT_SECRET in .env
docker compose --profile full up --build -d
```

Then open `http://localhost:3000`. Type the `http://` — nothing here serves
HTTPS, and browsers upgrade a bare `localhost:3000` to `https://`.

Once it is behind a domain and a reverse proxy, that address is whatever you put
in front of it. Pages in this handbook that need to name this instance write
`{{instance.url}}`, which resolves to **{{instance.url}}** as you are reading
it — so the same handbook is correct on every deployment.

## What must be in the environment

Only two things, and both are infrastructure rather than product settings:

| Variable | Why it has to be here |
|---|---|
| `DEVFORGE_DB_URL` and credentials | The application needs a database before it can read anything, including its own settings |
| `DEVFORGE_JWT_SECRET` | Signs access tokens. Minimum 32 characters; the application refuses to start below that |

The committed default secret is a public string in the repository. Anyone who has
read the source could mint valid tokens for a deployment still using it. Generate
your own before the instance is reachable by anyone else.

Everything a person would recognise as a setting — the name, the mark, who may
sign up, whether documentation can be published — is chosen in setup and stored in
the database. It survives a redeploy, and changing it needs no shell access.

## First run

An instance nobody has claimed redirects every route to `/setup` and shows
nothing else. See **First-run setup** for what the four steps decide.

## Upgrading

Migrations run at startup, so an upgrade is a redeploy. The instance settings row
survives it.

Back up **two** things. Everything DevForge stores lives in PostgreSQL — including
uploaded logos, deliberately — except the git repositories it hosts, which are
packfiles on disk:

```bash
pg_dump -U devforge devforge > devforge.sql
tar czf devforge-git.tgz /opt/docker/appdata/devforge/git
```

A repository can be reconstructed by pushing again, so losing one is recoverable
where losing the database is not — as long as somebody still has a clone.
