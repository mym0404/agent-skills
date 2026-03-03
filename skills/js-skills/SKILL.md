---
name: js-skills
description: >
  Zod schema definition patterns for domain model validation.
  Use when defining Zod schemas, creating domain models, or integrating
  const assertion enums with Zod validation.
license: MIT
metadata:
  author: mj
  version: "2.0.0"
---

# JS Skills — Zod Schema Patterns

Consistent Zod schema definition rules extracted from production domain modeling.

## When to Apply

Reference these guidelines when:

- Defining new domain model schemas with Zod
- Extending base schemas for entity models
- Integrating const assertion enums with Zod validation
- Composing or deriving schemas from existing ones
- Creating model instances with auto-generated base fields

## Rule Categories by Priority

| Priority | Category           | Impact | Prefix        |
| -------- | ------------------ | ------ | ------------- |
| 1        | Schema Definition  | HIGH   | `definition-` |
| 2        | Schema Usage       | HIGH   | `usage-`      |

## Quick Reference

### 1. Schema Definition (HIGH)

- `definition-naming-convention` — Consistent naming for schemas, types, and files
- `definition-type-inference` — Derive types from schemas with z.infer
- `definition-const-enum` — Const assertion arrays + z.enum integration

### 2. Schema Usage (HIGH)

- `usage-schema-composition` — Compose schemas via nesting, pick, partial
- `usage-create-model` — Use createModel for instance creation with auto base fields

## How to Use

Read individual rule files for details and examples:

```
rules/definition-naming-convention.md
rules/definition-type-inference.md
rules/definition-const-enum.md
rules/usage-schema-composition.md
rules/usage-create-model.md
```

Each rule file contains:

- Why the rule matters
- Incorrect implementation example
- Correct implementation example
- Reference links

## Full Compiled Document

For the complete expanded guide: `AGENTS.md`
