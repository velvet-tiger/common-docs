---
name: common-docs-write-all
description: Write complete, substantive content for every documentation file in the standard Common docs/ structure. Run common-docs-scaffold first if the docs/ directory does not exist. Use when asked to "write all the docs", "populate all documentation", "fill in all docs", "write the full docs suite", or "document everything". Distinct from common-docs-write (single file) and common-docs-scaffold (empty stubs only).
---

# Common Docs Write All

Write fully populated content for every file in the standard `docs/` structure in one pass.

Read `specification.md` in the common-docs repository before writing any files — it defines the required content for every file type.

---

## Pre-flight: Scaffold Check

Before writing content, verify that the directory structure exists.

1. Check whether `docs/` exists in the project root.
2. If `docs/` is **missing or empty**, run the `common-docs-scaffold` skill first to create the directory structure and stub files.
3. If `docs/` already has stub files (frontmatter only, no real content), proceed directly to writing.
4. If any file already has substantive content (more than frontmatter + a heading), **skip it** — never overwrite existing content.

---

## Workflow

### Phase 1: Gather Context

Before writing a single file, collect everything available in the project:

1. Read `README.md` — project name, purpose, technology stack, install/run commands.
2. Read `AGENTS.md` and/or `CLAUDE.md` — conventions, commands, architectural notes.
3. List the project root — identify languages, frameworks, config files (`.env.example`, `docker-compose.yml`, `Makefile`, `package.json`, `Cargo.toml`, etc.).
4. List source directories — understand service boundaries, module structure.
5. Read CI pipeline files (`.github/workflows/`, `Jenkinsfile`, `.circleci/`) — deployment targets, test commands, environments.
6. Read migration files or ORM schema files — understand the data model.
7. Read test directories — understand test suites and coverage expectations.
8. Read any existing `docs/` content — avoid contradicting what is already written.

Batch all of this discovery upfront. Do not interleave discovery with writing.

### Phase 2: Ask What You Cannot Find

After exploration, if critical facts are missing, ask the user in a **single batched message**. Examples of things to ask:

- Stakeholders and their roles (if not in any file)
- RTO/RPO targets (if no disaster recovery doc exists)
- Escalation contacts for runbooks
- Roadmap horizon and ownership
- Third-party integrations not mentioned in code

Do not ask about things discoverable from the codebase. One question batch only.

### Phase 3: Write All Files

Write every file in the order below. After completing each section, briefly note what was written before moving to the next.

Use the writing standards from `common-docs-write` for all files:
- **Present tense** for architecture, context, and reference docs
- **Imperative mood** for guides and runbooks
- **Mermaid diagrams** for `architecture/overview.md`, `architecture/data-model.md`, `architecture/infrastructure.md`
- **Tables** for env vars, services, endpoints, alerts, error codes, stakeholders
- **Numbered steps** in all guides and runbooks — never bulleted
- Never invent specific values (URLs, credentials, service names) not found in the codebase

#### Writing Order

Write files in this sequence so that each file can reference what came before:

**1. Context** (understand the domain first)
- `docs/context/product.md`
- `docs/context/domain.md`
- `docs/context/stakeholders.md`
- `docs/context/index.md`

**2. Architecture** (describe the present system)
- `docs/architecture/overview.md` — include a Mermaid system diagram
- `docs/architecture/data-model.md` — include a Mermaid ER or class diagram
- `docs/architecture/services.md`
- `docs/architecture/infrastructure.md`
- `docs/architecture/api-design.md`
- `docs/architecture/constraints.md`
- `docs/architecture/index.md`

**3. Configuration**
- `docs/configuration/environment.md` — table of all env vars found in `.env.example` or config files
- `docs/configuration/feature-flags.md`
- `docs/configuration/secrets.md`
- `docs/configuration/index.md`

**4. API** (if the project exposes an API)
- `docs/api/endpoints.md`
- `docs/api/authentication.md`
- `docs/api/errors.md`
- `docs/api/rate-limiting.md`
- `docs/api/changelog.md`
- `docs/api/index.md`

**5. Security**
- `docs/security/threat-model.md`
- `docs/security/auth.md`
- `docs/security/data-handling.md`
- `docs/security/vulnerability-management.md`
- `docs/security/incident-response.md`
- `docs/security/index.md`

**6. Guides**
- `docs/guides/getting-started.md`
- `docs/guides/development.md`
- `docs/guides/testing.md`
- `docs/guides/deployment.md`
- `docs/guides/contributing.md`
- `docs/guides/index.md`

**7. Operations**
- `docs/operations/monitoring.md`
- `docs/operations/disaster-recovery.md`
- `docs/operations/capacity-planning.md`
- `docs/operations/runbooks/deploy.md`
- `docs/operations/runbooks/rollback.md`
- `docs/operations/runbooks/scale.md`
- `docs/operations/runbooks/incident.md`
- `docs/operations/runbooks/index.md`
- `docs/operations/index.md`

**8. Plans** (future work — write last, as it references current state)
- `docs/plans/roadmap.md`
- `docs/plans/epics/index.md`
- `docs/plans/features/index.md`
- `docs/plans/index.md`

**9. ADR** (decisions log)
- `docs/adr/template.md` — use the exact template from the specification
- `docs/adr/index.md` — empty table ready to receive ADRs

**10. Integrations** (one file per third-party service found in code)
- `docs/integrations/<service-name>.md` for each discovered integration
- `docs/integrations/index.md`

**11. Migrations**
- `docs/migrations/index.md` — table of any migration files found in the project

**12. Changelog**
- `docs/changelog/index.md`

**13. Master index** (last, so it can accurately link everything)
- `docs/index.md`

---

## Content Quality Rules

- **Never leave placeholder text.** Instead of "TODO: fill this in", write `> Not yet defined — update when X is decided.`
- **Every file must have complete frontmatter** with `title`, `description`, `status: draft`, and `updated: YYYY-MM-DD` (today's date).
- **Section index files** must list every file in the section with a one-line description.
- **Guides and runbooks** must have numbered steps — not bullet points.
- **Diagrams** in architecture files must be valid Mermaid syntax.
- **Tables** for environment variables must cover every variable found in `.env.example` or config files.

---

## Skipping a Section

If a section genuinely does not apply (e.g. no API for a CLI tool, no migrations for a stateless service), write the `index.md` with a brief explanation of why the section is empty rather than skipping the file entirely.

Example:
```markdown
> This project does not expose an HTTP API. This section is retained for structural consistency.
```

---

## After Writing

1. Update `docs/index.md` to accurately reflect all sections written.
2. Confirm the `AGENTS.md` / `CLAUDE.md` reference block is present (add it if missing — see `common-docs-scaffold` for the block content).
3. Report a summary: how many files were written, which were skipped (already had content), and which sections were marked as not applicable.
