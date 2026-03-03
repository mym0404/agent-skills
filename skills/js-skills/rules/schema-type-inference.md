---
title: Type Inference from Schema
impact: HIGH
impactDescription: Single source of truth — types always stay in sync with validation rules
tags: zod, schema, type, inference, infer
---

## Type Inference from Schema

Always derive TypeScript types from Zod schemas using `z.infer`. Never define types separately from their schema.

**Incorrect:**

```typescript
import { z } from 'zod';

export const batchSchema = z.object({
  type: z.enum(['active', 'inactive']),
  batchNumber: z.string(),
});

// Manually duplicated type — drifts from schema over time
export type Batch = {
  type: 'active' | 'inactive';
  batchNumber: string;
};
```

Why it fails: Type and schema can diverge when one is updated but not the other. Doubles maintenance burden.

**Correct:**

```typescript
import { z } from 'zod';
import { baseModelSchema } from './Base';

export const batchSchema = baseModelSchema.extend({
  type: z.enum(['active', 'inactive']),
  batchNumber: z.string(),
});

export type Batch = z.infer<typeof batchSchema>;
```

For derived types from schema keys, use `keyof`:

```typescript
export const aptitudeScoresSchema = z.object({
  voca: z.int(),
  math: z.int(),
  science: z.int(),
});

export type AptitudeScores = z.infer<typeof aptitudeScoresSchema>;
export type AptitudeType = keyof AptitudeScores;
```

Reference:
[Zod Type Inference](https://zod.dev/?id=type-inference)
