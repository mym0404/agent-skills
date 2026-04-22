# AGENTS.md

This file provides guidance to coding agents working in this repository.

## Repository Overview

This repository stores agent skills in a consistent, rule-based structure.
Each skill is a self-contained directory under `skills/` with documentation, rules, and metadata.

There are no build, lint, or test commands — this is a pure Markdown + JSON documentation repo.

## Skill Invocation Policy

All skills in this repository must remain explicit-invocation only.
If a skill includes `agents/openai.yaml`, set `policy.allow_implicit_invocation: false` and keep it false on every update.

## Current Skills

| Skill | Version | Rules | Description |
|-------|---------|-------|-------------|
| `electron-skills` | 1.0.2 | 2 | Type-safe Electron IPC architecture |
| `js-skills` | 2.9.4 | 6 | Zod, dayjs, `@mj-studio/js-util`, class-based modules, Zustand, and feature-first architecture |
| `react-skills` | 2.1.1 | 3 | React data fetching plus `@mj-studio/react-util` guide-driven refactoring |
| `style-js-ts` | 1.0.1 | 0 | JavaScript/TypeScript and React JSX style guide |

## Directory Structure

```text
skills/
  {skill-name}/
    SKILL.md          # Skill overview, rule list, quick reference
    metadata.json     # Version, abstract, references
    rules/
      _sections.md    # Section ordering and prefix definitions
      _template.md    # Template for new rules
      {prefix}-{rule-name}.md   # Individual rule files (source of truth)
```

## CRITICAL: File Synchronization Rule

When adding, removing, or modifying a skill, **ALL of the following files MUST be updated together**:

| File | What to sync |
|------|-------------|
| `SKILL.md` | Rule list in Quick Reference, version in frontmatter |
| `metadata.json` | Version number, abstract, references |
| `rules/_sections.md` | Section definitions and prefixes |
| `rules/{prefix}-{rule-name}.md` | Actual rule files (source of truth) |

### Sync checklist (run after every change)

- [ ] `name` in `SKILL.md` frontmatter matches the parent directory name
- [ ] Every rule listed in `SKILL.md` Quick Reference has a corresponding file in `rules/`
- [ ] Every rule file in `rules/` (excluding `_sections.md`, `_template.md`) is listed in `SKILL.md`
- [ ] Every rule prefix used in rule filenames is defined in `rules/_sections.md`
- [ ] `metadata.json` `version` matches `SKILL.md` frontmatter `metadata.version`
- [ ] No orphaned rule files (file exists but not referenced anywhere)
- [ ] No phantom references (listed in `SKILL.md` but file doesn't exist)

**Violation of sync = broken skill. Agents MUST verify sync before finishing any skill modification.**

## Creating a New Skill

### 1. Create directory structure

```bash
mkdir -p skills/{skill-name}/rules
```

### 2. Create all required files

- `SKILL.md` — frontmatter + rule list
- `metadata.json` — version and references
- `rules/_sections.md` — section definitions
- `rules/_template.md` — rule template
- `rules/{prefix}-{rule-name}.md` — at least one rule

### 3. Verify sync checklist above

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Skill directory | `kebab-case` | `electron-skills` |
| `SKILL.md` | Exact uppercase filename | `SKILL.md` |
| Rule files | `{prefix}-{rule-name}.md` | `ipc-type-safe-architecture.md` |
| Rule prefixes | Defined in `_sections.md` | `ipc-`, `definition-`, `usage-` |

## SKILL.md Format

```markdown
---
name: {skill-name}
description: {what this skill does and when to use it}
license: MIT
metadata:
  author: {author}
  version: "1.0.0"
---
```

## Authoring Rules

Each rule file MUST include:

- YAML frontmatter (`title`, `impact`, `impactDescription`, `tags`)
- Clear problem statement
- **Incorrect** example with explanation of why it fails
- **Correct** example with explanation of why it's better
- Reference links

### Style guidelines

- Write all rule content in English
- Keep `SKILL.md` concise — put deep guidance in `rules/*.md`
- Use explicit relative file references from `SKILL.md`
- One independent pattern = one rule file
- If multiple concepts are interdependent parts of the same architecture, keep them in a single rule file
- Don't split a single cohesive pattern into multiple rules

### Rule granularity

**Separate rules** when patterns are independently applicable:

```
definition-naming-convention.md   # Can use without knowing type-inference
definition-type-inference.md      # Can use without knowing naming-convention
```

**Single rule** when patterns form one cohesive flow:

```
ipc-type-safe-architecture.md     # API type → bridge → handler → registration = one flow
```

## Modifying an Existing Skill

1. Make the content change (add/edit/delete rule files)
2. Update `SKILL.md` Quick Reference to reflect current rules
3. Bump `version` in both `SKILL.md` frontmatter and `metadata.json` if changing rules
4. Update `rules/_sections.md` if adding/removing prefixes
5. Run sync checklist
6. Update the Current Skills table in this root `AGENTS.md` if version or rule count changed

## Deleting a Skill

1. Remove the entire `skills/{skill-name}/` directory
2. Update the Current Skills table in this root `AGENTS.md`
