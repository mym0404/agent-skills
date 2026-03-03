---
title: Schema Naming Convention
impact: HIGH
impactDescription: Keeps schema/type/file naming predictable and searchable across the codebase
tags: zod, schema, naming, convention
---

## Schema Naming Convention

Follow consistent naming rules for schemas, types, and files.

**Incorrect:**

```typescript
// File: basic_record.ts
export const BasicRecordSchema = z.object({ ... });
export type BasicRecordType = z.infer<typeof BasicRecordSchema>;
```

Why it fails: PascalCase schema name collides with type convention. `Type` suffix is redundant. snake_case filename breaks project convention.

**Correct:**

```typescript
// File: BasicRecord.ts
export const basicRecordSchema = baseModelSchema.extend({
  sequence: z.int(),
  name: z.string(),
  sn: z.string(),
});

export type BasicRecord = z.infer<typeof basicRecordSchema>;
```

Rules:

| Element       | Convention                | Example                 |
| ------------- | ------------------------- | ----------------------- |
| File name     | PascalCase                | `BasicRecord.ts`        |
| Schema const  | camelCase + `Schema`      | `basicRecordSchema`     |
| Inferred type | PascalCase (= file name)  | `BasicRecord`           |
| Directory     | `domain/model/`           | `src/domain/model/`     |

One schema per file. The file name matches the exported type name.

Reference:
[Zod Documentation](https://zod.dev)
