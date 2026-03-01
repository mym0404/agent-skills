---
title: Prefer Guard Clauses
impact: HIGH
impactDescription: Reduces nesting and cognitive load
tags: javascript, readability, control-flow
---

## Prefer Guard Clauses

Use early returns for invalid or edge cases before the main path.

**Incorrect:**

```javascript
const parseCount = (value) => {
  if (value !== null && value !== undefined) {
    const parsed = Number(value)
    if (!Number.isNaN(parsed)) {
      return parsed
    }
  }

  return 0
}
```

**Correct:**

```javascript
const parseCount = (value) => {
  if (value === null || value === undefined) {
    return 0
  }

  const parsed = Number(value)
  if (Number.isNaN(parsed)) {
    return 0
  }

  return parsed
}
```

Reference:
[MDN Number.isNaN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN)
