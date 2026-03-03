---
title: Const Assertion Enum with Zod
impact: HIGH
impactDescription: Enables runtime iteration and compile-time safety without duplicating enum values
tags: zod, enum, const-assertion, union-type
---

## Const Assertion Enum with Zod

Define enum-like values as `const` assertion arrays, then integrate with `z.enum()`. Never duplicate literal values between a type and a schema.

**Incorrect:**

```typescript
// Values duplicated in two places
type Status = 'pending' | 'active' | 'inactive';

const statusSchema = z.enum(['pending', 'active', 'inactive']);

// No way to iterate over valid values at runtime
```

Why it fails: Literal values are duplicated — adding a new status requires updating two locations. No runtime array to iterate or validate against.

**Correct:**

```typescript
// src/domain/types/Status.ts
export const allStatuses = ['pending', 'active', 'inactive'] as const;
export type Status = (typeof allStatuses)[number];
```

```typescript
// In schema file — reference the const array
import { allStatuses } from '@domain/types/Status';

export const recordSchema = baseModelSchema.extend({
  status: z.enum(allStatuses),
});
```

Naming rules:

| Element    | Convention                    | Example           |
| ---------- | ----------------------------- | ----------------- |
| Array      | `all` + PascalCase plural     | `allStatuses`     |
| Type       | PascalCase singular           | `Status`          |
| Location   | `domain/types/`              | `Status.ts`       |

Runtime usage:

```typescript
// Iteration
allStatuses.forEach(status => console.log(status));

// Type guard
const isValidStatus = (value: string): value is Status =>
  allStatuses.includes(value as Status);
```

Reference:
[Zod Enums](https://zod.dev/?id=zod-enums)
[TypeScript const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)
