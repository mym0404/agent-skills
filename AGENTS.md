# AGENTS.md

This file provides guidance to coding agents working in this repository.

## Repository Overview

This repository stores agent skills in a consistent, rule-based structure.

## Creating a New Skill

### Directory Structure

```text
skills/
  {skill-name}/
    SKILL.md
    AGENTS.md
    metadata.json
    rules/
      _sections.md
      _template.md
      {prefix}-{rule-name}.md
```

### Naming Conventions

- Skill directory: `kebab-case` (for example, `js-skills`)
- `SKILL.md`: uppercase and exact filename
- `AGENTS.md`: uppercase and exact filename
- Rule files: `{prefix}-{rule-name}.md`
- Rule prefixes must be defined in `rules/_sections.md`

### SKILL.md Format

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

### Authoring Rules

Each rule file should include:

- Clear problem statement
- Incorrect example and why it fails
- Correct example and why it is safer/faster
- Reference links

### Context Efficiency

- Keep `SKILL.md` concise
- Put deep guidance in `rules/*.md`
- Use explicit relative file references from `SKILL.md`

### Validation Checklist

- `name` in `SKILL.md` matches parent directory name
- Every rule listed in `SKILL.md` exists in `rules/`
- Every rule prefix exists in `rules/_sections.md`
- `metadata.json` version matches `SKILL.md` metadata version
