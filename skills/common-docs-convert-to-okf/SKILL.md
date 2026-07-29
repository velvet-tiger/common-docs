---
name: common-docs-convert-to-okf
description: Convert an existing Common docs/ tree to be OKF (Open Knowledge Format) v0.2 conformant, by adding the required `type` frontmatter field and fixing index.md frontmatter. Use when asked to "convert docs to OKF", "make this OKF compatible", "retrofit for OKF", "update docs for OKF conformance", or when a pre-1.1 common-docs docs/ tree needs to catch up to the current spec. Distinct from common-docs-write/write-all (content authoring) — this skill only patches metadata for conformance, it doesn't rewrite content.
---

# Common Docs → OKF Converter

Retrofit an existing `docs/` tree — scaffolded or written under common-docs spec v1.0, or any version pre-dating OKF compatibility — so it conforms to [OKF v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md), per the "OKF Compatibility" section of the specification.

Read `[references/specification.md](https://github.com/velvet-tiger/common-docs/blob/main/specification.md)` before converting anything — in particular the Frontmatter Standard, Type values table, and OKF Compatibility sections.

This skill is a **metadata patch, not a content rewrite**. It never touches body content except stripping `index.md`'s frontmatter block, and it never invents facts. For content authoring or backfilling the optional OKF fields with real values, use `common-docs-write` or `common-docs-write-all` instead.

---

## Scope check

Before converting, confirm `docs/` exists. If it doesn't, this skill doesn't apply — suggest `common-docs-scaffold` instead.

OKF conformance needs exactly two things fixed, per the specification's two hard requirements:
1. Every non-reserved `.md` file has a non-empty `type` field.
2. Every `index.md` has no frontmatter, except `docs/index.md` (the bundle root), which carries `okf_version: "0.2"` only.

---

## Workflow

1. **Inventory** — walk `docs/` recursively. Classify every `.md` file as either reserved (`index.md`, `log.md`) or a concept file.
2. **Check idempotency per file** — a concept file that already has a non-empty `type` needs no change; an `index.md` that already has no frontmatter (or, for the root, already has exactly `okf_version`) needs no change. Skip these silently — don't touch files that are already conformant, and don't re-run this on a tree that's already been converted.
3. **Plan the changes** — build a list of: concept files needing `type` added (with the value each will get), index.md files needing frontmatter stripped, and any file whose path doesn't match a known pattern (see "Unrecognized files" below). Show this plan to the user before applying it if the tree is large (more than ~10 files) or contains unrecognized files; for a small, fully-recognized tree, proceed directly.
4. **Apply — concept files:** add `type: <value>` to the existing frontmatter block, using the table below. Don't touch `title`, `description`, `status`, `updated`, `authors`, `related`, or the body — insert only the one key.
5. **Apply — index.md files:** remove the frontmatter block entirely (delimiters and all), leaving the body untouched. For `docs/index.md` specifically, replace whatever frontmatter it currently has with exactly:
   ```yaml
   ---
   okf_version: "0.2"
   ---
   ```
6. **Report** — summarize what changed: how many files got `type` added, how many `index.md` files had frontmatter stripped, and list any files that received a fallback type (see below) so the user can review and tighten them.

---

## Type values

| Path pattern | `type` value |
|---|---|
| `context/*.md` | `Context` |
| `architecture/*.md` | `Architecture` |
| `adr/NNNN-*.md` | `ADR` |
| `adr/template.md` | `Template` |
| `plans/roadmap.md` | `Plan` |
| `plans/epics/*.md` | `Epic` |
| `plans/features/*.md` | `Feature` |
| `api/*.md` | `API` |
| `configuration/*.md` | `Configuration` |
| `integrations/*.md` | `Integration` |
| `security/*.md` | `Security` |
| `guides/*.md` | `Guide` |
| `operations/*.md` (top-level files) | `Operations` |
| `operations/runbooks/*.md` | `Runbook` |
| `migrations/*.md` | `Migration` |
| `changelog/*.md` | `Changelog` |

### Unrecognized files

If a project has added files outside the standard structure (a custom section, an extra file in a known section), no row above applies. Fall back to the Title-Cased parent directory name (e.g. a stray file in `docs/design/` gets `type: Design`), flag it in the report, and suggest the user confirm or refine the value — never leave `type` empty just because nothing matched.

---

## Optional fields (not part of this skill's core pass)

This skill doesn't add `tags`, `resource`, `generated`, `sources`, or `stale_after` — those need real content judgement, not a mechanical patch, and adding them without genuine values would mean fabricating data. After conversion, offer to run `common-docs-write` against specific files if the user wants those backfilled properly.

---

## After Converting

1. Spot-check a couple of converted files to confirm `type` was added without disturbing anything else.
2. Confirm no `index.md` other than the root still has a frontmatter block.
3. Report the summary from step 6, and flag any fallback-typed files for the user's review.
