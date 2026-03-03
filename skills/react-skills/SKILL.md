---
name: react-skills
description: >
  React best practices for data fetching. Covers server-first loading in Server
  Components and client-side Suspense patterns with AsyncBoundary + TanStack Query.
license: MIT
metadata:
  author: mj
  version: "2.0.0"
---

# React Skills

Ruleset for React data fetching patterns.

## Rule Categories by Priority

| Priority | Category          | Impact | Prefix  |
| -------- | ----------------- | ------ | ------- |
| 1        | Data Fetching     | HIGH   | `data-` |

## Quick Reference

- `data-server-first-fetching` - Fetch data in Server Components first
- `data-async-boundary-suspense-query` - Use AsyncBoundary with useSuspenseQuery for client data fetching

## How to Use

Read the rule files:

```
rules/data-server-first-fetching.md
rules/data-async-boundary-suspense-query.md
```
