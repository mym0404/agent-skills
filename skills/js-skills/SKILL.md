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

| Priority | Category         | Impact | Prefix    |
| -------- | ---------------- | ------ | --------- |
| 1        | Schema Patterns  | HIGH   | `schema-` |

## Quick Reference

### 1. Schema Patterns (HIGH)

- `schema-naming-convention` — Consistent naming for schemas, types, and files
- `schema-type-inference` — Derive types from schemas with z.infer
- `schema-const-enum` — Const assertion arrays + z.enum integration
- `schema-composition` — Compose schemas via nesting, pick, partial
- `schema-create-model` — Use createModel for instance creation with auto base fields

## How to Use

Read individual rule files for details and examples:

```
rules/schema-naming-convention.md
rules/schema-type-inference.md
rules/schema-const-enum.md
rules/schema-composition.md
rules/schema-create-model.md
```

Each rule file contains:

- Why the rule matters
- Incorrect implementation example
- Correct implementation example
- Reference links

## Full Compiled Document

For the complete expanded guide: `AGENTS.md`
