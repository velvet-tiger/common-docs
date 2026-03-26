# Automatic Project Documentation Structure Specification

> **Version:** 1.0  
> **Status:** Draft  
> **Purpose:** Defines the standard structure, naming conventions, and content expectations for the `docs/` directory across all projects. Designed to serve both human contributors and AI coding agents.

---

## Principles

1. **Context before detail.** Directories are ordered so that a reader (human or agent) builds understanding progressively — domain first, then design, then implementation, then operations.
2. **Every directory has an `index.md`.** It is the entry point for that section and must list and briefly describe all files within it. Agents use `index.md` files for navigation without scanning the full tree.
3. **One concern per file.** Files cover a single topic. When a file exceeds ~300 lines, split it.
4. **Files are named in `kebab-case`, lowercase.** No spaces, no underscores except in ADR/migration prefixes.
5. **Each file opens with a YAML frontmatter block.** Minimum: `title`, `description`, `status`, `updated`. This enables index auto-generation.
6. **Plans describe the future. ADRs record the past. Architecture describes the present.** These three must never be conflated.
7. **`docs/` is not a wiki.** It is curated, versioned, and maintained alongside code. Stale documentation must be updated or deleted — it is worse than no documentation.
8. **`CLAUDE.md` / `AGENTS.md` at the repo root must reference `docs/index.md`** as the starting point for project understanding.

---

## Frontmatter Standard

Every `.md` file in `docs/` must open with:

```yaml
---
title: Short descriptive title
description: One sentence describing what this document contains.
status: draft | active | deprecated | superseded
updated: YYYY-MM-DD
authors:
  - Name
related:
  - path/to/related.md
---
```

`status` meanings:
- `draft` — work in progress, not authoritative
- `active` — current and maintained
- `deprecated` — no longer applies but kept for history
- `superseded` — replaced by another document (link in `related`)

---

## Directory Structure

```
docs/
├── index.md                        # Master index — entry point for everything
│
├── context/                        # Why we exist, who we serve, domain vocabulary
│   ├── index.md
│   ├── product.md                  # Vision, goals, target users, value proposition
│   ├── domain.md                   # Domain model, terminology, ubiquitous language
│   ├── competitive.md              # Market context and positioning
│   └── stakeholders.md             # Who cares about what, decision-making structure
│
├── architecture/                   # Current system design (present tense)
│   ├── index.md
│   ├── overview.md                 # Narrative + high-level diagram of the system
│   ├── data-model.md               # Entities, relationships, storage decisions
│   ├── api-design.md               # API style, versioning, auth patterns
│   ├── infrastructure.md           # Cloud resources, networking, deployment topology
│   ├── services.md                 # Service inventory, responsibilities, interfaces
│   └── constraints.md              # Known limitations, scale ceilings, tech debt
│
├── adr/                            # Architecture Decision Records (immutable log)
│   ├── index.md                    # Table of all ADRs with status
│   ├── template.md                 # Blank ADR to copy
│   ├── 0001-example-decision.md
│   └── 0002-example-decision.md
│
├── plans/                          # Future work (roadmap, epics, feature specs)
│   ├── index.md
│   ├── roadmap.md                  # High-level timeline and priority order
│   ├── epics/                      # Large bodies of work spanning multiple features
│   │   ├── index.md
│   │   └── EPIC-001-name.md
│   └── features/                   # Individual feature specs (PRD-style)
│       ├── index.md
│       └── feature-name.md
│
├── api/                            # API reference
│   ├── index.md
│   ├── endpoints.md                # Route inventory with methods, params, responses
│   ├── authentication.md           # How to authenticate, token lifecycle
│   ├── errors.md                   # Error codes, messages, retry guidance
│   ├── rate-limiting.md
│   └── changelog.md                # API version history
│
├── configuration/                  # All configuration reference
│   ├── index.md
│   ├── environment.md              # All env vars: name, type, default, required, purpose
│   ├── feature-flags.md            # Flags, their states, and expected behaviour
│   └── secrets.md                  # What secrets exist, where they're stored, rotation policy
│                                   # (no actual secret values — reference only)
│
├── integrations/                   # Per third-party system documentation
│   ├── index.md
│   └── <service-name>.md           # Auth model, endpoints used, failure modes, gotchas
│
├── security/                       # Security posture and practices
│   ├── index.md
│   ├── threat-model.md             # Assets, threats, mitigations
│   ├── auth.md                     # AuthN/AuthZ design and flows
│   ├── data-handling.md            # PII classification, retention, encryption at rest/transit
│   ├── vulnerability-management.md # How vulns are reported, triaged, and resolved
│   └── incident-response.md        # Playbook for security incidents
│
├── guides/                         # How-to for humans (and agents doing setup)
│   ├── index.md
│   ├── getting-started.md          # First-time setup from zero
│   ├── development.md              # Day-to-day dev workflow, tooling, conventions
│   ├── deployment.md               # How to ship to each environment
│   ├── testing.md                  # How to run tests, what each suite covers
│   └── contributing.md             # PR process, review expectations, code standards
│
├── operations/                     # Running the system in production
│   ├── index.md
│   ├── monitoring.md               # Metrics, dashboards, alert definitions
│   ├── disaster-recovery.md        # RTO/RPO targets, recovery procedures
│   ├── capacity-planning.md        # Scale assumptions, growth triggers
│   └── runbooks/                   # Step-by-step operational procedures
│       ├── index.md
│       ├── deploy.md
│       ├── rollback.md
│       ├── scale.md
│       └── incident.md
│
├── migrations/                     # Database and data migration log
│   ├── index.md
│   └── YYYYMMDD-description.md     # One file per significant migration
│
└── changelog/                      # Release and change history
    ├── index.md
    └── YYYY-MM-DD-vX.Y.Z.md        # One file per release
```

---

## File Specifications

### `docs/index.md`

The master entry point. An AI agent or new contributor should be able to read this and know exactly where to go for any question.

**Must contain:**
- One-paragraph project summary (what it is, what it does, who uses it)
- Ordered directory listing with one-line descriptions
- Quick-reference section: how to run locally, how to deploy, where to find X
- Links to `CLAUDE.md` / `AGENTS.md` and `README.md`

---

### `context/index.md`

**Must contain:**
- Why this section exists and what "context" means in this project
- Links to all files with one-line descriptions
- A note on keeping this section updated as the product evolves

### `context/product.md`

**Must contain:**
- Problem statement
- Target users and their jobs-to-be-done
- Value proposition
- Success metrics
- Out of scope (explicitly)

### `context/domain.md`

The ubiquitous language document. Agents read this to avoid misusing terminology.

**Must contain:**
- Glossary of domain terms (alphabetical)
- Key domain concepts and how they relate
- Terms that sound similar but mean different things
- External standards or specifications the domain is based on (e.g. DICOM, FHIR, HL7)

### `context/stakeholders.md`

**Must contain:**
- Who the key stakeholders are (roles, not names where possible)
- What each stakeholder cares about
- Who makes which kinds of decisions

---

### `architecture/index.md`

**Must contain:**
- Links to all files with one-line descriptions
- A note that this describes **current state**, not intended future state (see `plans/`)
- Date of last architecture review

### `architecture/overview.md`

**Must contain:**
- A system diagram (Mermaid preferred — renders in GitHub and is diffable)
- Narrative walkthrough of the diagram
- Key system properties (e.g. "stateless", "event-driven", "multi-tenant")
- Links to more detailed docs for each component

### `architecture/constraints.md`

**Must contain:**
- Known technical debt with impact assessment
- Hard scale limits
- External constraints (regulatory, contractual, platform)
- Things that cannot be changed without a major version

---

### `adr/index.md`

**Must contain:**
A table:

| ID | Title | Status | Date |
|----|-------|--------|------|
| 0001 | Use Rust for proxy layer | accepted | 2024-03-01 |

Statuses: `proposed` | `accepted` | `deprecated` | `superseded`

### `adr/template.md`

```markdown
---
title: "ADR-NNNN: Title"
status: proposed
date: YYYY-MM-DD
authors:
  - Name
supersedes: ~
superseded-by: ~
---

# ADR-NNNN: Title

## Status

Proposed | Accepted | Deprecated | Superseded by [ADR-XXXX](XXXX-title.md)

## Context

What situation, problem, or constraint requires a decision?
What forces are at play? What are the constraints?

## Decision

What did we decide to do?
State it clearly and directly.

## Consequences

### Positive
- 

### Negative
- 

### Risks
- 

## Alternatives Considered

### Option A: Name
Why this was not chosen.

### Option B: Name
Why this was not chosen.

## References
- 
```

**ADR rules:**
- ADRs are **never edited** once accepted — only superseded
- A new ADR must be written to reverse or change a previous decision
- Numbering is sequential and never reused
- The ADR file name format is `NNNN-short-title.md` (zero-padded to 4 digits)

---

### `plans/index.md`

**Must contain:**
- Current roadmap summary (link to `roadmap.md`)
- Table of active epics with status
- Table of active feature specs with status and epic link

### `plans/roadmap.md`

**Must contain:**
- Timeline horizon (e.g. "this covers the next 6 months")
- Ordered list of priorities with rough timeframes
- What is explicitly not planned and why
- How this roadmap is maintained and who owns it

### `plans/epics/EPIC-NNN-name.md`

**Must contain:**
- Problem being solved
- Success criteria
- Scope (in/out)
- List of constituent features (links to `plans/features/`)
- Status: `planned` | `in-progress` | `complete` | `cancelled`

### `plans/features/feature-name.md`

**Must contain:**
- Problem statement
- User stories or requirements
- Acceptance criteria (testable)
- Technical design notes (or link to ADR)
- Tasks / implementation checklist
- Status: `spec` | `in-progress` | `complete` | `cancelled`

---

### `configuration/environment.md`

**Must contain:**
A table for every environment variable:

| Variable | Type | Default | Required | Description |
|----------|------|---------|----------|-------------|
| `DATABASE_URL` | string | — | yes | PostgreSQL connection string |
| `LOG_LEVEL` | enum | `info` | no | `debug`, `info`, `warn`, `error` |

No actual secret values. Reference where they're stored (e.g. "GCP Secret Manager — `projects/foo/secrets/bar`").

---

### `integrations/<service-name>.md`

**Must contain:**
- What we use this service for
- Authentication method and where credentials are stored
- Key endpoints or operations we use
- Known failure modes and how we handle them
- Rate limits or quotas
- Runbook link if operational procedures exist

---

### `operations/runbooks/index.md`

**Must contain:**
Table of all runbooks with: name, trigger condition, estimated duration, last tested date.

### `operations/runbooks/<name>.md`

**Must contain:**
- **Trigger:** When to use this runbook
- **Prerequisites:** Access, tools, context needed before starting
- **Steps:** Numbered, atomic, unambiguous
- **Verification:** How to confirm the procedure succeeded
- **Rollback:** What to do if it fails
- **Contacts:** Who to escalate to

---

### `migrations/YYYYMMDD-description.md`

**Must contain:**
- Date and description
- Schema or data changes made
- How to run it
- How to reverse it
- Whether it is destructive or requires downtime

---

## `AGENTS.md` / `CLAUDE.md` Reference Block

Add this to your `AGENTS.md` or `CLAUDE.md`:

```markdown
## Documentation

All project documentation is in `docs/`. Always start at `docs/index.md`.

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

---

## Maintenance

- Every PR that changes behaviour should update the relevant `docs/` files in the same commit
- ADRs are written when a significant technical decision is made — not retroactively in bulk
- `docs/index.md` is reviewed at the start of each planning cycle
- Stale documents (status `deprecated` for more than 6 months with no reference) are deleted
- Architecture docs are reviewed against actual system state at least once per quarter
