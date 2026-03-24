---
title: Schema Definition Patterns
impact: HIGH
impactDescription: Keeps Zod schema definitions current, consistent, composable, and aligned with domain model creation
tags: zod, schema, naming, composition, inference, createModel, zod4
---

## Schema Definition Patterns

Use one cohesive schema flow: verify current Zod APIs with Context7, define schemas with current helpers, infer types from schemas, reuse meaningful fragments, and create instances through schema-validated model factories.

### Naming and Type Inference

Follow consistent naming for schema files, schema constants, and inferred types. Never define a duplicate TypeScript type beside the schema.

**Incorrect:**

```typescript
// File: basic_record.ts
export const BasicRecordSchema = z.object({
  sequence: z.int(),
  name: z.string(),
});

export type BasicRecordType = {
  sequence: number;
  name: string;
};
```

Why it fails: The file name is inconsistent, the schema naming does not match project convention, and the duplicated type can drift from the schema.

**Correct:**

```typescript
// File: BasicRecord.ts
export const basicRecordSchema = z.object({
  sequence: z.int(),
  name: z.string(),
});

export type BasicRecord = z.infer<typeof basicRecordSchema>;
```

Rules:

- File name uses PascalCase such as `BasicRecord.ts`.
- Schema constant uses camelCase plus `Schema`, such as `basicRecordSchema`.
- Inferred type uses PascalCase and matches the file name, such as `BasicRecord`.
- Keep one schema per file when that file represents a named domain model.

### Latest Zod Definition API

Check Context7 before introducing or changing schema definition patterns. Prefer current Zod 4 helpers over deprecated chains. Keep simple format validators inline, and avoid extra strictness or transformation unless there is a concrete business requirement.

**Incorrect:**

```typescript
const recordIdSchema = z.string().uuid();
const createdAtSchema = z.string().datetime();
const callbackUrlSchema = z.string().url();

export const webhookSchema = z
  .object({
    id: recordIdSchema,
    createdAt: createdAtSchema,
    callbackUrl: callbackUrlSchema,
    tags: z.array(z.string()).min(1),
  })
  .strict()
  .transform(value => ({
    ...value,
    createdAt: dayjs(value.createdAt),
  }));
```

Why it fails: It uses deprecated format validators, extracts trivial single-use validators, adds strictness and length constraints without domain justification, and converts serialized datetime values at the schema boundary.

**Correct:**

```typescript
const webhookEventSchema = z.object({
  type: z.string(),
  payload: z.string(),
});

export const webhookSchema = z.object({
  id: z.uuid(),
  createdAt: z.iso.datetime(),
  callbackUrl: z.url(),
  tags: z.array(z.string()).optional(),
  events: z.array(webhookEventSchema).optional(),
});
```

Rules:

- Prefer `z.url()`, `z.uuid()`, and `z.iso.datetime()` over deprecated chains such as `z.string().url()`, `z.string().uuid()`, and `z.string().datetime()`.
- Keep simple field-level format validators inline. Do not extract single-use schemas such as `const urlSchema = z.url()`.
- Extract repeated, meaningful schema fragments and reuse them.
- Do not default to `z.strictObject()` or `.strict()`. Use strict object validation only when unknown keys are a real business error.
- Do not add constraints such as `.min(1)` unless the domain explicitly requires them.
- Use `preprocess`, `transform`, `pipe`, and `coerce` only when the input source genuinely requires normalization or coercion.
- Validate serialized datetimes with `z.iso.datetime()`. Do not model schema boundary values as `Date`, `Dayjs`, or codec-driven wrappers.

### Reusable Enum-Like Values

When literal values must be shared between runtime code and a schema, define them once as a const assertion array and derive both the Zod schema and the TypeScript type from that source.

**Incorrect:**

```typescript
type Status = 'pending' | 'active' | 'inactive';

const statusSchema = z.enum(['pending', 'active', 'inactive']);
```

Why it fails: The literal values are duplicated and can drift when one side changes first.

**Correct:**

```typescript
export const allStatuses = ['pending', 'active', 'inactive'] as const;
export type Status = (typeof allStatuses)[number];

export const recordSchema = z.object({
  status: z.enum(allStatuses),
});
```

### Composition and Reuse

Compose schemas through nesting, extending, picking, and partial derivation. Do not re-list the same fields across related schemas.

**Incorrect:**

```typescript
export const personalRecordOmrSchema = z.object({
  name: z.string().max(10),
  xrayCode: z.int(),
  jumin: z.string().regex(/^\d{6}-\d{7}$/),
  highSchool: z.int().optional(),
});
```

Why it fails: Re-listing fields duplicates validation logic and makes schema drift likely.

**Correct:**

```typescript
export const personalRecordOmrSchema = personalRecordSchema
  .pick({
    name: true,
    xrayCode: true,
    jumin: true,
    highSchool: true,
  })
  .partial();
```

```typescript
const detailedScoreSchema = z.object({
  aptitudeScore: z.number().optional(),
  licenseScore: z.number().optional(),
  totalScore: z.number().optional(),
});

export const divisionPreferenceSchema = baseModelSchema.extend({
  basicRecordId: z.uuid(),
  firstPreferenceScores: detailedScoreSchema.optional(),
  secondPreferenceScores: detailedScoreSchema.optional(),
});
```

### Model Creation

Instantiate domain entities through `createModel` so schema validation and base field generation happen in one path.

**Incorrect:**

```typescript
const record = {
  id: crypto.randomUUID(),
  createdAt: new Date().toISOString(),
  sequence: 1,
  name: 'John',
  sn: '12345678',
};
```

Why it fails: Base field creation is duplicated and the instance bypasses schema validation.

**Correct:**

```typescript
const record = createModel(basicRecordSchema, {
  sequence: 1,
  name: 'John',
  sn: '12345678',
  xrayCode: 100,
  division: 'engineering',
  jumin: '900101-1234567',
});
```

```typescript
export const createModel = <T extends ZodType>(
  schema: T,
  value: Omit<z.infer<T>, keyof BaseModel>,
) => {
  return schema.parse(value);
};
```

Reference:
[Zod 4](https://zod.dev/v4)
[Zod API](https://zod.dev/api)
[Zod Type Inference](https://zod.dev/?id=type-inference)
[TypeScript const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)
