---
name: style-js-ts
description: >
  JavaScript/TypeScript and React JSX style rules extracted from the global
  AGENTS.md. Use when writing or refactoring JS/TS code and deciding function
  shape, type design, exports, JSX formatting, naming, comments, constants,
  or low-cognitive-load structure.
license: MIT
metadata:
  author: mj
  version: "1.0.1"
---

# Style JS TS

Single-file style guide for JavaScript, TypeScript, and React JSX code.

## When to Apply

Reference this guide when:

- writing or refactoring JavaScript or TypeScript
- deciding function parameter shape or helper extraction
- choosing type shapes, aliases, or narrowing strategy
- deciding export style or whether to add `index.ts`
- formatting React JSX props or repeated element rendering
- reviewing code for readability and cognitive load

## Readability

- Prefer the simplest structure that keeps intent obvious.
- Reuse existing aliases or enums when they already express the domain.
- One-off literals can stay inline, but repeated magic strings or numbers should become named constants.
- Avoid unnecessary abstraction layers, indirection, or extra options.
- Use plain objects or arrays unless `Map` or `Set` is clearly needed.
- Do not rewrite already-correct code just to add stylistic churn.

**Incorrect:**

```typescript
const createStatusChecker = () => {
  const store = new Map<string, string>([
    ["active", "active"],
    ["pending", "pending"],
  ])

  return {
    isActive: (status: string) => {
      return status === "active" || store.get(status) === "active"
    },
  }
}
```

**Correct:**

```typescript
const ACTIVE_STATUS = "active"

const isActiveStatus = (status: string) => {
  return status === ACTIVE_STATUS
}
```

## Naming And Comments

- Use clear identifier names.
- Keep comments minimal and in English.
- Do not add comments that restate obvious code.
- Do not quote user prompts or chat context in code comments.
- Do not use icons as decorative signal in code-facing text.

**Incorrect:**

```typescript
// User asked to hide admin-only data
const x = (a: User[]) => {
  return a.filter((item) => item.isVisible)
}
```

**Correct:**

```typescript
const filterVisibleUsers = (users: User[]) => {
  return users.filter((user) => user.isVisible)
}
```

## Function Patterns

- Prefer arrow functions.
- Prefer a single object parameter with destructuring when a function has multiple related fields.
- Keep simple one- or two-argument functions positional when that shape is clearer.
- Prefer extracted helpers over dense inline logic.
- Prefer array methods such as `map`, `filter`, and `some` over manual loops when expressing repeated transforms.
- Keep control flow simple and easy to understand in one pass.

**Incorrect:**

```typescript
function createUser(name: string, email: string, role: "admin" | "member") {
  return { name, email, role }
}

const getVisibleNames = (users: User[]) => {
  const result: string[] = []

  for (const user of users) {
    if (user.deletedAt) {
      continue
    }

    if (user.name.trim().length === 0) {
      continue
    }

    result.push(user.name.trim())
  }

  return result
}
```

**Correct:**

```typescript
const createUser = ({
  name,
  email,
  role,
}: {
  name: string
  email: string
  role: "admin" | "member"
}) => {
  return { name, email, role }
}

const isVisibleUser = (user: User) => {
  return !user.deletedAt && user.name.trim().length > 0
}

const getVisibleNames = (users: User[]) => {
  return users.filter(isVisibleUser).map((user) => user.name.trim())
}
```

## Type Design

- Prefer inferred variable and return types.
- Use `Type[]` instead of `Array<Type>`.
- Keep one-off function parameter shapes inline.
- Extract a `type` alias when a shape is long or reused.
- Prefer `type` aliases over `interface`.
- Avoid excessive standalone type declarations when inline types are enough.
- Prefer `undefined` over `null` unless `null` has real domain meaning.

**Incorrect:**

```typescript
interface UserProfile {
  name: string
  tags: Array<string>
}

const getDisplayName = (profile: UserProfile): string => {
  return profile.name ?? null
}
```

**Correct:**

```typescript
type UserProfile = {
  name: string
  tags: string[]
}

const getDisplayName = (profile: UserProfile) => {
  return profile.name
}
```

## Type Safety

- Do not use `any`.
- Do not use `as any` or `as unknown as`.
- Do not use `@ts-ignore`, `@ts-nocheck`, or `@ts-expect-error`.
- Use `unknown` only at real external boundaries such as `catch` or unchecked input.
- Do not use `unknown` for ordinary function parameters that should have concrete types.
- When types are genuinely unclear, stop and confirm the expected shape instead of forcing a cast.

**Incorrect:**

```typescript
// @ts-ignore
const parseUser = (value: unknown) => {
  return (value as any).name as string
}
```

**Correct:**

```typescript
const getErrorMessage = (error: unknown) => {
  if (error instanceof Error) {
    return error.message
  }

  return "Unknown error"
}
```

## Module Boundaries

- Prefer named exports over default exports.
- Do not add export-only `index.ts` barrel files.

**Incorrect:**

```typescript
const createUserStore = () => {
  return {}
}

export default createUserStore
```

```typescript
// user/index.ts
export * from "./UserCard"
export * from "./UserTable"
```

**Correct:**

```typescript
export const createUserStore = () => {
  return {}
}
```

```typescript
import { UserCard } from "./UserCard"
import { UserTable } from "./UserTable"
```

## React JSX

- Wrap string prop literals with braces.
- Render repeated elements with `map`.

**Incorrect:**

```tsx
<Button variant="primary" size="sm" />

return (
  <ul>
    <li>{users[0].name}</li>
    <li>{users[1].name}</li>
    <li>{users[2].name}</li>
  </ul>
)
```

**Correct:**

```tsx
<Button variant={"primary"} size={"sm"} />

return (
  <ul>
    {users.map((user) => (
      <li key={user.id}>{user.name}</li>
    ))}
  </ul>
)
```

## Summary

- Prefer direct, low-cognitive-load code.
- Prefer arrow functions and clear parameter shapes.
- Prefer inference-first TypeScript with lightweight `type` aliases.
- Ban unsafe type escapes and TS suppression comments.
- Prefer named exports and avoid barrel `index.ts`.
- Keep React JSX prop literals and list rendering consistent.

Reference:
[Cognitive Load](https://github.com/zakirullin/cognitive-load)
[TypeScript Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
[TypeScript Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
[MDN export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
[React JSX Curly Braces](https://react.dev/learn/javascript-in-jsx-with-curly-braces)
[React Rendering Lists](https://react.dev/learn/rendering-lists)
