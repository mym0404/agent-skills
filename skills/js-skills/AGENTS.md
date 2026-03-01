# JS Skills

**Version 1.0.0**
Engineering
March 2026

---

## Abstract

Minimal JavaScript guidance for readable and maintainable logic.

---

## Table of Contents

1. [Language Patterns](#1-language-patterns) - **HIGH**
   - 1.1 [Prefer Guard Clauses](#11-prefer-guard-clauses)

---

## 1. Language Patterns

**Impact: HIGH**

### 1.1 Prefer Guard Clauses

**Impact: HIGH (reduces nesting and cognitive load)**

Use early returns to keep branches shallow and readable.

**Incorrect:**

```javascript
const formatUserLabel = (user) => {
  if (user) {
    if (user.name) {
      return user.name.trim()
    }
  }
  return "Unknown"
}
```

**Correct:**

```javascript
const formatUserLabel = (user) => {
  if (!user || !user.name) {
    return "Unknown"
  }

  return user.name.trim()
}
```

Reference:
[MDN if...else](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)
