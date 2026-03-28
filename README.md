# Common Docs

A standard structure, naming conventions, and content expectations for the `docs/` directory across all projects. Designed to serve both human contributors and AI coding agents.

## Overview

This specification defines how to organize project documentation using a progressive disclosure model:

- **Context** → **Architecture** → **Implementation** → **Operations**

Every file has a purpose. Every directory has an entry point. Documentation is versioned alongside code — not scattered across wikis or buried in Notion.

## The Specification

The [Specification](specification.md) describes the directory structure and how to use the format.

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

## Skills

This project includes three AI agent skills for working with Common Docs:

### common-docs-scaffold

Scaffolds the standard `docs/` directory structure in a project. Creates all section directories and stub files with correct frontmatter.

**Use when:** Setting up docs, scaffolding documentation, creating the docs structure, or adding docs to a project.

**Usage:** `/common-docs-scaffold`

The skill will:
1. Check what exists in the project root
2. Ask which sections to create (default: all)
3. Create files using the content spec in `references/specification.md`
4. Add the documentation reference table to `AGENTS.md` or `CLAUDE.md`

### common-docs-write

Writes the actual content of a specific documentation file. Runs a structured interview for the target file type, explores the codebase to fill in what it can, then produces a complete document meeting the spec's "Must contain" requirements.

**Use when:** Writing, filling in, or populating a doc — e.g. "write the architecture overview", "fill in the threat model", "write the getting started guide", "document our API endpoints", "write an ADR for X".

**Usage:** `/common-docs-write`

The skill will:
1. Identify the target file and look up its content requirements in the spec
2. Ask a set of targeted questions specific to that file type
3. Explore the codebase to fill gaps without bothering you
4. Write the complete file — no TODOs or placeholders
5. Offer to update related files (index, ADR index, etc.)

### common-docs-find

Finds the right documentation file for any topic using the standard Common Docs structure. Maps questions and topics to canonical file paths.

**Use when:** Looking for where to find something, where to document something, which file covers a topic, or before reading/writing docs to know the correct location.

**Usage:** `/common-docs-find`

The skill will identify the topic from your question and map it to the correct file path using its lookup table.

### Installing Skills

Skills are available in two locations:
- `skills/` — Packaged skills for distribution (referenced by `skill.json`)

## Third-Party Support

Common Docs is available as a plugin [Automatic](https://tryautomatic.app).

## Status

This specification is versioned and maintained.

## License

MIT License - see [LICENSE](LICENSE)
