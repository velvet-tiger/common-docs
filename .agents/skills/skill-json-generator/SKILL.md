---
name: skill-json-generator
description: Generate skill.json package metadata files for AI agent skill packages. Use this skill whenever the user wants to create, edit, or validate a skill.json file, when they're packaging skills for distribution, when they mention "skill.json", "skill manifest", "skill metadata", "skill package", or when they have a directory of SKILL.md files and need to make them discoverable by package managers (skmr, skillman, skillbox, skills-manifest, skills-supply, etc.). Also use when the user is publishing skills to a registry, preparing a skill repo for sharing, or wants to add integrity hashes to existing skill packages.
---

# skill.json Generator

Generate and maintain `skill.json` package metadata files that work with any skill package manager in the ecosystem.

## What is skill.json?

`skill.json` is a **publisher-side** metadata file that lives at the root of a skill package (a repo, directory, or archive containing one or more AI agent skills). It tells package managers what skills exist, where to find them, and provides enough metadata to index, search, verify, and install them.

It does **not** declare which agents to install to (that's the installer's job), or which skills a consumer project needs (that's a consumer manifest concern).

## When to Use This Skill

- User says "create a skill.json" or "package my skills"
- User has SKILL.md files and wants to distribute them
- User wants to publish skills to a registry
- User needs to validate or update an existing skill.json
- User mentions any skill package manager (skmr, skillman, skillbox, skills-supply, skills-manifest)
- User is preparing a repo that contains skills for sharing

## Workflow

### Step 1: Discover Skills

First, understand what we're working with. Scan the project for existing SKILL.md files:

```bash
find . -name "SKILL.md" -type f | head -50
```

For each SKILL.md found, read the YAML frontmatter to extract `name`, `description`, and `license`:

```bash
head -20 path/to/SKILL.md
```

Also check for an existing skill.json to determine if we're creating fresh or updating:

```bash
test -f skill.json && cat skill.json || echo "No existing skill.json"
```

### Step 2: Gather Package-Level Metadata

Ask the user for anything that can't be inferred. Prioritise inferring from the environment first:

**Can usually be inferred:**
- `name` — From the directory/repo name
- `skills` array — From discovered SKILL.md files
- `repository` — From `.git/config` if present
- Per-skill `path`, `name`, `description` — From SKILL.md frontmatter and location

**Must ask the user for:**
- `version` — Unless there's a tag, existing skill.json, or package.json to read from
- `author` — Unless inferable from git config or package.json
- `license` — Unless present in SKILL.md frontmatter or a LICENSE file

**Optional but valuable (suggest to user):**
- `keywords` — Package-level search terms
- `homepage` — Documentation URL
- Per-skill `category`, `tags` — For registry discoverability
- Per-skill `requires.tools` — Runtime dependencies like `python3`, `node`

### Step 3: Generate the File

Read `references/schema-spec.md` for the complete schema specification before generating.

Build the skill.json following these rules:

**Required fields at package level:**
- `$schema` — Always include for validation support
- `name` — Lowercase, hyphens allowed, e.g. `"my-skill-pack"`
- `version` — Semver, e.g. `"1.0.0"`
- `description` — One-line package summary
- `skills` — Array with at least one skill entry

**Required fields per skill:**
- `name` — Must match directory name, lowercase with hyphens
- `path` — Relative from skill.json to the skill directory (use `"."` for root-level single skills)
- `description` — What it does AND when to use it

**Include when available:**
- `author` — Object `{ "name", "url", "email" }` or npm shorthand `"Name <email> (url)"`
- `license` — SPDX identifier or `"SEE LICENSE"`
- `repository` — Object `{ "type": "git", "url" }` or URL shorthand string
- `integrity` — SRI hash (generate with the script in `scripts/`)
- `category` — One of: `development`, `documents`, `creative`, `business`, `productivity`, `infrastructure`, `data`, `testing` (or custom)
- `tags` — Specific search terms relevant to the skill
- `requires.tools` — Runtime tools the skill needs
- `requires.min_agent_versions` — Only if there's a genuine compatibility constraint
- `dependencies` — Only for intra-package skill dependencies

### Step 4: Compute Integrity Hashes (Optional but Recommended)

Run the integrity hash script for each skill:

```bash
python3 scripts/compute_integrity.py path/to/skill-directory
```

This produces an SRI-format hash (`sha256-...`) that goes in the skill's `integrity` field. The hash changes when any file in the skill directory is added, removed, or modified — enabling verification and cache-based deduplication by package managers.

### Step 5: Validate

Validate the generated file against the JSON Schema:

```bash
python3 scripts/validate.py skill.json
```

Or validate inline with Python:

```python
import json, jsonschema

with open('skill.schema.json') as f:
    schema = json.load(f)
with open('skill.json') as f:
    data = json.load(f)

jsonschema.validate(data, schema)
```

### Step 6: Present to User

Write the file to the project root and present it. Explain:
- What each section means
- Which fields they might want to customise
- How to regenerate integrity hashes after editing skills
- Which package managers can now consume it

## Common Patterns

### Single Skill at Repo Root

When a repo IS the skill (SKILL.md at root):

```json
{
  "$schema": "https://skill.json.org/schema/1.0.0/skill.schema.json",
  "name": "my-skill",
  "version": "1.0.0",
  "description": "What this skill does",
  "skills": [
    {
      "name": "my-skill",
      "path": ".",
      "description": "Detailed description of what the skill does and when to use it"
    }
  ]
}
```

### Multi-Skill Collection

Skills in subdirectories:

```json
{
  "$schema": "https://skill.json.org/schema/1.0.0/skill.schema.json",
  "name": "my-skill-pack",
  "version": "1.0.0",
  "description": "Collection of related skills",
  "skills": [
    {
      "name": "skill-a",
      "path": "./skill-a",
      "description": "..."
    },
    {
      "name": "skill-b",
      "path": "./skill-b",
      "description": "..."
    }
  ]
}
```

### Monorepo Subdirectory

When skills live in a subdirectory of a larger repo:

```json
{
  "name": "org-skills",
  "version": "1.0.0",
  "description": "Our internal skills",
  "repository": {
    "type": "git",
    "url": "https://github.com/org/monorepo.git",
    "directory": "packages/skills"
  },
  "skills": [...]
}
```

### Skill With Runtime Requirements

When a skill needs specific tools installed:

```json
{
  "name": "pdf-skill",
  "path": "./pdf",
  "description": "PDF manipulation",
  "requires": {
    "tools": ["python3"]
  }
}
```

### Skills With Internal Dependencies

When one skill in the package requires another:

```json
{
  "skills": [
    {
      "name": "base-patterns",
      "path": "./base-patterns",
      "description": "Foundation patterns"
    },
    {
      "name": "advanced-patterns",
      "path": "./advanced-patterns",
      "description": "Advanced patterns building on base",
      "dependencies": ["base-patterns"]
    }
  ]
}
```

## Package Manager Compatibility

The generated skill.json works with all major skill package managers:

| Tool | What it reads |
|------|--------------|
| **skmr** | `skills[].name` for manifest entries |
| **skillman** | `skills[].name` for `{ source, skills: [...] }` |
| **skillbox** | `skills[].name` + `skills[].path` for install |
| **skills-manifest** | `skills[].name` for its map format |
| **skills-supply** | `skills[].name` + `skills[].path` for discovery |
| **skills-detector** | `skills[].tags` + `skills[].category` for matching |
| **MCP Registry** | `name`, `version`, `description`, `repository` for entries |

## Reference Files

- `references/schema-spec.md` — Complete schema specification with all field types, constraints, and design rationale
- `references/skill.schema.json` — JSON Schema file for validation
- `scripts/compute_integrity.py` — Compute SRI integrity hashes for skill directories
- `scripts/validate.py` — Validate a skill.json against the schema
