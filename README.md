# Automatic Documentation Structure Specification

A standard structure, naming conventions, and content expectations for the `docs/` directory across all projects. Designed to serve both human contributors and AI coding agents.

## Overview

This specification defines how to organize project documentation using a progressive disclosure model:

- **Context** → **Architecture** → **Implementation** → **Operations**

Every file has a purpose. Every directory has an entry point. Documentation is versioned alongside code — not scattered across wikis or buried in Notion.

## Quick Start

1. Copy the `docs/` structure into your project
2. Fill in `docs/index.md` with your project summary
3. Work through sections in order: `context/` → `architecture/` → the rest
4. Add the reference block to your `AGENTS.md` or `CLAUDE.md`

## Directory Structure

```
docs/
├── index.md                    # Master entry point
├── context/                    # Why we exist, who we serve, domain vocabulary
├── architecture/               # Current system design
├── adr/                        # Architecture Decision Records
├── plans/                      # Future work, roadmap, feature specs
├── api/                        # API reference
├── configuration/              # Configuration reference
├── integrations/               # Third-party integrations
├── security/                   # Security posture and practices
├── guides/                     # How-to guides for humans and agents
├── operations/                 # Production operations
├── migrations/                 # Database and data migrations
└── changelog/                  # Release history
```

## Key Principles

1. **Context before detail** — Build understanding progressively
2. **Every directory has an `index.md`** — Navigation without scanning
3. **One concern per file** — Split files over ~300 lines
4. **`kebab-case` naming** — No spaces, lowercase
5. **Frontmatter on every file** — title, description, status, updated
6. **Plans describe future. ADRs record past. Architecture describes present.** Never conflate them
7. **`docs/` is curated, not a wiki** — Stale docs are deleted
8. **Reference `docs/index.md` from `AGENTS.md`** — Agents start there

## Status

This specification is versioned and maintained. Changes follow the same ADR process as technical decisions.

## License

MIT License - see [LICENSE](LICENSE)
