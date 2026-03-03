# JS Skills — Zod Schema Patterns

**Version 2.0.0**
Engineering
March 2026

---

## Abstract

Zod schema definition patterns for domain model validation, composition, and instance creation.

---

## Table of Contents

1. [Schema Definition](#1-schema-definition) - **HIGH**
   - 1.1 [Schema Naming Convention](#11-schema-naming-convention)
   - 1.2 [Type Inference from Schema](#12-type-inference-from-schema)
   - 1.3 [Const Assertion Enum with Zod](#13-const-assertion-enum-with-zod)
2. [Schema Usage](#2-schema-usage) - **HIGH**
   - 2.1 [Schema Composition](#21-schema-composition)
   - 2.2 [Create Model Instances](#22-create-model-instances)

---

## 1. Schema Definition

**Impact: HIGH**

### 1.1 Schema Naming Convention

**Impact: HIGH (keeps schema/type/file naming predictable and searchable)**

| Element       | Convention                | Example                 |
| ------------- | ------------------------- | ----------------------- |
| File name     | PascalCase                | `BasicRecord.ts`        |
| Schema const  | camelCase + `Schema`      | `basicRecordSchema`     |
| Inferred type | PascalCase (= file name)  | `BasicRecord`           |
| Directory     | `domain/model/`           | `src/domain/model/`     |

**Incorrect:**

```typescript
// File: basic_record.ts
export const BasicRecordSchema = z.object({ ... });
export type BasicRecordType = z.infer<typeof BasicRecordSchema>;
```

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

One schema per file. The file name matches the exported type name.

Reference:
[Zod Documentation](https://zod.dev)

---

### 1.2 Type Inference from Schema

**Impact: HIGH (single source of truth — types always stay in sync with validation rules)**

Always derive TypeScript types from Zod schemas using `z.infer`. Never define types separately.

**Incorrect:**

```typescript
export const batchSchema = z.object({
  type: z.enum(['active', 'inactive']),
  batchNumber: z.string(),
});

export type Batch = {
  type: 'active' | 'inactive';
  batchNumber: string;
};
```

**Correct:**

```typescript
export const batchSchema = baseModelSchema.extend({
  type: z.enum(['active', 'inactive']),
  batchNumber: z.string(),
});

export type Batch = z.infer<typeof batchSchema>;
```

For derived types from schema keys:

```typescript
export type AptitudeScores = z.infer<typeof aptitudeScoresSchema>;
export type AptitudeType = keyof AptitudeScores;
```

Reference:
[Zod Type Inference](https://zod.dev/?id=type-inference)

---

### 1.3 Const Assertion Enum with Zod

**Impact: HIGH (enables runtime iteration and compile-time safety without duplicating enum values)**

Define enum-like values as `const` assertion arrays, then integrate with `z.enum()`.

**Incorrect:**

```typescript
type Status = 'pending' | 'active' | 'inactive';
const statusSchema = z.enum(['pending', 'active', 'inactive']);
```

**Correct:**

```typescript
// src/domain/types/Status.ts
export const allStatuses = ['pending', 'active', 'inactive'] as const;
export type Status = (typeof allStatuses)[number];
```

```typescript
// In schema file
import { allStatuses } from '@domain/types/Status';

export const recordSchema = baseModelSchema.extend({
  status: z.enum(allStatuses),
});
```

| Element    | Convention                    | Example           |
| ---------- | ----------------------------- | ----------------- |
| Array      | `all` + PascalCase plural     | `allStatuses`     |
| Type       | PascalCase singular           | `Status`          |
| Location   | `domain/types/`              | `Status.ts`       |

Reference:
[Zod Enums](https://zod.dev/?id=zod-enums)
[TypeScript const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

---

## 2. Schema Usage

**Impact: HIGH**

### 2.1 Schema Composition

**Impact: HIGH (reuses existing schemas without duplication)**

Compose schemas by nesting, extending, picking, or making partial.

**Incorrect:**

```typescript
export const personalRecordOmrSchema = z.object({
  name: z.string().max(10),
  xrayCode: z.int(),
  jumin: z.string().regex(/^\d{6}-\d{7}$/),
});
```

**Correct — Nesting:**

```typescript
export const aptitudeRecordSchema = baseModelSchema.extend({
  name: z.string(),
  xrayCode: z.int(),
  aptitudeScores: aptitudeScoresSchema,
});
```

**Correct — Pick + Partial:**

```typescript
export const personalRecordOmrSchema = personalRecordSchema
  .pick({ name: true, xrayCode: true, jumin: true, highSchool: true })
  .partial();

export type PersonalRecordOmr = z.infer<typeof personalRecordOmrSchema>;
```

**Correct — Internal helper schemas:**

```typescript
const detailedScoreSchema = z.object({
  aptitudeScore: z.number().optional(),
  licenseScore: z.number().optional(),
  totalScore: z.number().optional(),
});

export const divisionPreferenceSchema = baseModelSchema.extend({
  firstPreferenceScores: detailedScoreSchema.optional(),
  secondPreferenceScores: detailedScoreSchema.optional(),
});
```

Non-exported helper schemas stay file-scoped. Only export if reused across files.

Reference:
[Zod .pick()](https://zod.dev/?id=pick) |
[Zod .partial()](https://zod.dev/?id=partial) |
[Zod .extend()](https://zod.dev/?id=extend)

---

### 2.2 Create Model Instances

**Impact: HIGH (centralizes instance creation with auto-generated base fields and schema validation)**

Use `createModel` to instantiate domain entities. It auto-generates `id` and `createdAt`, then validates.

**Incorrect:**

```typescript
import { v4 } from 'uuid';

const record = {
  id: v4(),
  createdAt: new Date(),
  sequence: 1,
  name: 'John',
  sn: '12345678',
};
```

**Correct:**

```typescript
import { createModel } from '@domain/model/Base';
import { basicRecordSchema } from '@domain/model/BasicRecord';

const record = createModel(basicRecordSchema, {
  sequence: 1,
  name: 'John',
  sn: '12345678',
  xrayCode: 100,
  division: 'engineering',
  jumin: '900101-1234567',
});
```

The `createModel` function:

```typescript
export const createModel = <T extends ZodType>(
  schema: T,
  value: Omit<z.infer<T>, keyof BaseModel>,
) => {
  return schema.parse(value);
};
```

- `id` and `createdAt` are omitted from input — generated by schema defaults
- `schema.parse()` validates and throws `ZodError` on invalid data

Reference:
[Zod .parse()](https://zod.dev/?id=parse) |
[Zod .default()](https://zod.dev/?id=default)
