---
name: common-docs-write
description: Write the actual content of a specific documentation file following the Common docs/ specification. Use when asked to write, fill in, or populate a doc — e.g. "write the architecture overview", "fill in the threat model", "write the getting started guide", "document our API endpoints", "write an ADR for X". Distinct from scaffolding (which creates empty stubs) — this skill produces real, substantive content for a specific file.
---

# Common Docs Writer

Write a fully populated documentation file that meets the content requirements in the Common docs/ specification.

Read `[references/specification.md](https://github.com/velvet-tiger/common-docs/blob/main/specification.md)` before writing any files — it defines required content for every file.

---

## Workflow

1. **Identify the target file** — determine which `docs/` file is being written (use `common-docs-find` if uncertain).
2. **Check the spec** — look up the "Must contain" requirements for that file type in `references/specification.md`.
3. **Explore the codebase** — gather what you need before writing. Check: `README.md`, `AGENTS.md`, `CLAUDE.md`, existing `docs/`, config files, source structure, CI pipelines, migration files, test directories. Extract as much as possible without asking.
4. **Ask only what you cannot find** — if specific facts are not discoverable (e.g. who owns the roadmap, RTO targets, escalation contacts), ask for them. Batch all questions into one message.
5. **Write the file** — produce complete, substantive content. Never leave placeholder text like "TODO". If something is genuinely unknown, write `> Not yet defined — update when X is decided.` so the gap is explicit rather than hidden.
6. **Offer follow-ups** — after writing, suggest related files that may need updating (section `index.md`, `adr/index.md` if an ADR was written, etc.).

---

## Writing Standards

- **Present tense** for architecture, context, and reference docs
- **Imperative mood** for guides and runbooks ("Run `make dev`", not "You should run `make dev`")
- **Mermaid diagrams** for `architecture/overview.md`, `architecture/data-model.md`, `architecture/infrastructure.md`
- **Tables** for env vars, services, endpoints, alerts, error codes, stakeholders
- **Numbered steps** in all guides and runbooks — never bulleted
- Don't invent specific values (URLs, service names, credentials) not found in the codebase or provided by the user

---

## Frontmatter

Every file must open with:

```yaml
---
title: Short descriptive title
description: One sentence describing what this document contains.
status: draft
updated: YYYY-MM-DD
authors:
  - Name
related:
  - path/to/related.md
---
```

Use today's date for `updated`. Set `status: draft` unless told otherwise. Omit `authors` and `related` if not known.

---

## ADR Rules

- Never edit an accepted ADR — write a new one to supersede it
- Number sequentially from the highest existing (`0001`, `0002`, …); file name: `NNNN-short-kebab-title.md`
- After writing, add a row to `docs/adr/index.md`
- If superseding an existing ADR, set `superseded-by: NNNN-new-title.md` in the old ADR's frontmatter

---

## After Writing

- If this is a new file in a section, offer to update that section's `index.md`
- If a technical decision is embedded in the doc, offer to write a corresponding ADR
- If the file is a guide that references commands, verify those commands against the codebase
