---
title: Tech stack
type: TECH_STACK
internal: false
---

# Tech stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | React 19 + TypeScript, Vite | Fast builds, strict typing across the API boundary |
| Data fetching | TanStack Query | Cache invalidation as a first-class concern |
| Routing | React Router | Nested routes match the workspace shell |
| Backend | Java 21, Spring Boot 4 | Records, pattern matching, mature ecosystem |
| Persistence | Spring Data JPA + Hibernate | Repositories with hand-written queries where it matters |
| Migrations | Flyway | The schema is versioned and reviewable |
| Database | PostgreSQL 16 | Generated columns and full-text search built in |
| Auth | Spring Security, HMAC-signed JWT | Stateless; no session store to operate |
| Docs | springdoc-openapi | The spec is generated from the code |

## Testing

| Tool | Covers |
|---|---|
| JUnit 5, Mockito, AssertJ | Services with collaborators stubbed |
| Testcontainers | Integration tests against real PostgreSQL 16 |
| ArchUnit | Module boundaries, enforced at build time |
| Vitest + Testing Library | Components and hooks |

## Notable choices

**PostgreSQL does the search.** A generated `tsvector` column with a GIN index
means the index is part of each write and can never drift from the content.

**Modules talk through contracts.** Each feature module publishes interfaces and
records; nothing else about it is visible. ArchUnit fails the build if a module
reaches into another's internals.

**References are held by id, not by association.** A document stores a
`workspaceId`, not a `Workspace` object. Foreign keys still enforce integrity, but
the object graph does not span modules.
