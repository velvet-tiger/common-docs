# Agents

This is the **common-docs** project — a documentation structure specification for organizing project documentation. It defines the standard `docs/` directory layout, naming conventions, and content expectations used across all Automatic-managed projects.

## Project Purpose

This project exists to ensure consistent, navigable documentation across projects. The primary deliverable is `specification.md`, which defines the standard.

## Key Files

- `specification.md` — The full documentation structure specification
- `README.md` — Summary of the specification for human readers
- `docs/` — (when populated) Example implementation of the spec

## Working on This Project

### When modifying the specification

1. Changes to `specification.md` follow the same process as architectural decisions — write or update an ADR in `docs/adr/` if the change is significant
2. Update `README.md` if the summary changes
3. Test the spec by applying it to this project itself

### When documenting other projects

Follow the specification in `specification.md`. This project is the canonical reference implementation.

## Documentation

All project documentation is in `docs/`. Start at `docs/index.md` if it exists.

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
