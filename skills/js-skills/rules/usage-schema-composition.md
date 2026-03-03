---
title: Schema Composition
impact: HIGH
impactDescription: Reuses existing schemas without duplication — nesting, pick, partial keep schemas DRY
tags: zod, schema, composition, pick, partial, extend, nested
---

## Schema Composition

Compose schemas by nesting, extending, picking, or making partial. Never duplicate field definitions across schemas.

**Incorrect:**

```typescript
// Duplicating fields from PersonalRecord into a new schema
export const personalRecordOmrSchema = z.object({
  name: z.string().max(10),
  xrayCode: z.int(),
  jumin: z.string().regex(/^\d{6}-\d{7}$/),
  highSchool: z.int().optional(),
  // ... re-listing every field
});
```

Why it fails: Field definitions are duplicated. Changes to the source schema won't propagate. Validation rules can drift.

**Correct:**

### Nesting — embed one schema inside another

```typescript
import { aptitudeScoresSchema } from './AptitudeScore';
import { baseModelSchema } from './Base';

export const aptitudeRecordSchema = baseModelSchema.extend({
  name: z.string(),
  xrayCode: z.int(),
  sn: z.string(),
  aptitudeScores: aptitudeScoresSchema,
});
```

### Pick + Partial — derive a subset schema

```typescript
import { personalRecordSchema } from './PersonalRecord';

export const personalRecordOmrSchema = personalRecordSchema
  .pick({
    name: true,
    xrayCode: true,
    jumin: true,
    highSchool: true,
  })
  .partial();

export type PersonalRecordOmr = z.infer<typeof personalRecordOmrSchema>;
```

### Internal helper schemas — compose within a file

```typescript
const detailedScoreSchema = z.object({
  aptitudeScore: z.number().optional(),
  licenseScore: z.number().optional(),
  totalScore: z.number().optional(),
});

export const divisionPreferenceSchema = baseModelSchema.extend({
  basicRecordId: z.string().uuid(),
  firstPreferenceScores: detailedScoreSchema.optional(),
  secondPreferenceScores: detailedScoreSchema.optional(),
});
```

Non-exported helper schemas stay file-scoped. Only export if reused across files.

Reference:
[Zod .pick()](https://zod.dev/?id=pick)
[Zod .partial()](https://zod.dev/?id=partial)
[Zod .extend()](https://zod.dev/?id=extend)
