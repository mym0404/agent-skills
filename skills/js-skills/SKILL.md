---
name: js-skills
description: >
  Zod schema definition patterns, dayjs date/time handling conventions,
  and @mj-studio/js-util guide-driven refactoring rules.
  Use when defining Zod schemas, creating domain models, integrating
  const assertion enums with Zod validation, working with dayjs
  for date/time operations, or using @mj-studio/js-util functions.
license: MIT
metadata:
  author: mj
  version: "2.3.1"
---

# JS Skills — Zod Schema, Dayjs & JS Util Patterns

Consistent Zod schema definition rules, dayjs date/time conventions, and guide-driven utility refactoring rules extracted from production domain modeling.

## When to Apply

Reference these guidelines when:

- Defining new domain model schemas with Zod
- Extending base schemas for entity models
- Integrating const assertion enums with Zod validation
- Composing or deriving schemas from existing ones
- Creating model instances with auto-generated base fields
- Setting up dayjs plugins and locale in a project
- Working with timestamps, time arithmetic, formatting, or durations
- Using or reviewing public APIs from `@mj-studio/js-util`

## Rule Categories by Priority

| Priority | Category         | Impact | Prefix    |
| -------- | ---------------- | ------ | --------- |
| 1        | Schema Patterns  | HIGH   | `schema-` |
| 2        | Dayjs Patterns   | HIGH   | `dayjs-`  |
| 3        | JS Util Patterns | HIGH   | `js-util-` |

## Quick Reference

### 1. Schema Patterns (HIGH)

- `schema-naming-convention` — Consistent naming for schemas, types, and files
- `schema-type-inference` — Derive types from schemas with z.infer
- `schema-const-enum` — Const assertion arrays + z.enum integration
- `schema-composition` — Compose schemas via nesting, pick, partial
- `schema-create-model` — Use createModel for instance creation with auto base fields

### 2. Dayjs Patterns (HIGH)

- `dayjs-usage` — Plugin setup, timestamp ops, time arithmetic, formatting, duration

### 3. JS Util Patterns (HIGH)

- `js-util-usage` — Use upstream `llms.txt` to discover, apply, and refactor toward `@mj-studio/js-util` helpers instead of bespoke utility code

## How to Use

Read individual rule files for details and examples:

```
rules/schema-naming-convention.md
rules/schema-type-inference.md
rules/schema-const-enum.md
rules/schema-composition.md
rules/schema-create-model.md
rules/dayjs-usage.md
rules/js-util-usage.md
```

Each rule file contains:

- Why the rule matters
- Incorrect implementation example
- Correct implementation example
- Reference links
