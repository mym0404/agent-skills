# Sections

This file defines section ordering, impact levels, and filename prefixes.

---

## 1. Schema Patterns (schema)

**Impact:** HIGH
**Description:** Zod schema definition, composition, and instance creation patterns for domain modeling.

## 2. Dayjs Patterns (dayjs)

**Impact:** HIGH
**Description:** Dayjs plugin setup, timestamp operations, time arithmetic, formatting, and duration patterns.

## 3. JS Util Patterns (js-util)

**Impact:** HIGH
**Description:** Guide-driven rules for adopting `@mj-studio/js-util` helpers to simplify and refactor project code.

## 4. Module Patterns (module)

**Impact:** HIGH
**Description:** Prefer class-based modules over closure factories when a module owns mutable state and related methods.

## 5. Architecture Patterns (architecture)

**Impact:** HIGH
**Description:** Organize code by feature, keep shared code in `feature/common`, and nest submodules under the owning feature.
