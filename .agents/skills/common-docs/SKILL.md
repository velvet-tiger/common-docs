---
name: common-docs
description: Scaffold the standard Common docs/ directory structure in a project. Creates all section directories and stub files with correct frontmatter. Use when asked to "set up docs", "scaffold documentation", "create the docs structure", or "add docs to this project". Read references/specification.md for the full file-by-file content spec.
---

# Common Docs Scaffolder

Scaffold the standard `docs/` structure defined in the Common documentation specification.

Read `references/specification.md` before writing any files — it defines required content for every file.

## Workflow

1. **Check what exists** — list the project root. If `docs/` already exists, note which sections are present to avoid overwriting.
2. **Ask which sections to create** (if not told). Default is all. Sections: `context`, `architecture`, `adr`, `plans`, `api`, `configuration`, `integrations`, `security`, `guides`, `operations`, `migrations`, `changelog`.
3. **Create files** — write each file using the content spec in `references/specification.md`. Never overwrite an existing file.
4. **Add the AGENTS.md block** — append the documentation reference table to the project's `AGENTS.md` or `CLAUDE.md` if one exists.

## Directory Structure

```
docs/
├── index.md
├── context/
│   ├── index.md
│   ├── product.md
│   ├── domain.md
│   └── stakeholders.md
├── architecture/
│   ├── index.md
│   ├── overview.md
│   ├── data-model.md
│   ├── api-design.md
│   ├── infrastructure.md
│   ├── services.md
│   └── constraints.md
├── adr/
│   ├── index.md
│   └── template.md
├── plans/
│   ├── index.md
│   ├── roadmap.md
│   ├── epics/
│   │   └── index.md
│   └── features/
│       └── index.md
├── api/
│   ├── index.md
│   ├── endpoints.md
│   ├── authentication.md
│   ├── errors.md
│   ├── rate-limiting.md
│   └── changelog.md
├── configuration/
│   ├── index.md
│   ├── environment.md
│   ├── feature-flags.md
│   └── secrets.md
├── integrations/
│   └── index.md
├── security/
│   ├── index.md
│   ├── threat-model.md
│   ├── auth.md
│   ├── data-handling.md
│   ├── vulnerability-management.md
│   └── incident-response.md
├── guides/
│   ├── index.md
│   ├── getting-started.md
│   ├── development.md
│   ├── deployment.md
│   ├── testing.md
│   └── contributing.md
├── operations/
│   ├── index.md
│   ├── monitoring.md
│   ├── disaster-recovery.md
│   ├── capacity-planning.md
│   └── runbooks/
│       ├── index.md
│       ├── deploy.md
│       ├── rollback.md
│       ├── scale.md
│       └── incident.md
├── migrations/
│   └── index.md
└── changelog/
    └── index.md
```

## Frontmatter

Every file must open with:

```yaml
---
title: Short descriptive title
description: One sentence describing what this document contains.
status: draft
updated: YYYY-MM-DD
---
```

Use today's date for `updated`. Set `status: draft` on all stub files.

## AGENTS.md Block

After scaffolding, append this to the project's `AGENTS.md` or `CLAUDE.md`:

```markdown
## Documentation

All project documentation is in `docs/`. Start at `docs/index.md`.

| Section | Path | Contains |
|---------|------|----------|
| Domain & product context | `docs/context/` | Terminology, goals, stakeholders |
| System design | `docs/architecture/` | Current architecture, data model, infrastructure |
| Decision log | `docs/adr/` | Immutable record of architectural decisions |
| Future work | `docs/plans/` | Roadmap, epics, feature specs |
| API reference | `docs/api/` | Endpoints, auth, errors |
| Configuration | `docs/configuration/` | Env vars, flags, secrets reference |
| Integrations | `docs/integrations/` | Third-party system docs |
| Security | `docs/security/` | Threat model, auth design, data handling |
| How-to guides | `docs/guides/` | Setup, development, deployment, contributing |
| Production ops | `docs/operations/` | Monitoring, runbooks, disaster recovery |
| Migration log | `docs/migrations/` | DB and data migration history |
| Release history | `docs/changelog/` | What changed in each release |

When making architectural decisions, check `docs/adr/index.md` first.
When implementing a feature, check `docs/plans/features/` for a spec.
When unsure about terminology, check `docs/context/domain.md`.
```
