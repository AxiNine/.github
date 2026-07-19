# AxiNine

**Enterprise multi-tenant SaaS for Education & Government (B2B/B2G) in Thailand.**
Single codebase, multi-tenant by design — PostgreSQL (JWT + Row-Level Security), .NET 10 Clean Architecture, Nuxt 4 frontend, built for PDPA-grade governance.

## Repositories

| Repo | What it is |
|---|---|
| [`docs`](https://github.com/AxiNine/docs) | Platform architecture (v3.2 master + arc42 SAD) and Architecture Decision Records — start here |
| [`brand`](https://github.com/AxiNine/brand) | Brand Identity System + design tokens (published as `@axinine/design-tokens`) |
| [`contracts`](https://github.com/AxiNine/contracts) | API contracts (OpenAPI) — single source of truth, generates typed clients for frontend & mobile |
| [`backend`](https://github.com/AxiNine/backend) | .NET 10 API — Clean Architecture, CQRS, multi-tenancy (RLS), background jobs |
| [`frontend`](https://github.com/AxiNine/frontend) | Nuxt 4 web app — mirrors backend structure, consumes design tokens + API client |
| [`ai-worker`](https://github.com/AxiNine/ai-worker) | Python AI worker — consumes RabbitMQ events (AI Teacher Assistant, letter drafting, auto-grading) |
| [`mobile`](https://github.com/AxiNine/mobile) | Mobile super apps (Parent app, Health Volunteer app) |
| [`infra`](https://github.com/AxiNine/infra) | Local dev stack, Docker, Kubernetes (Phase 2), CI/CD |

## Conventions

- **Branching:** `main` (protected, releasable) + `develop` (integration); work on `feature/*` → PR → review → merge (§17.4).
- **Ownership:** core paths (auth, tenant engine, RLS, migrations) are locked via `CODEOWNERS`.
- **Zero-secret rule:** no credentials in source. Vault (runtime) + GitHub Secrets (CI) only (§16, §17.1).

*References to `§` are sections in the [platform architecture](https://github.com/AxiNine/docs).*
