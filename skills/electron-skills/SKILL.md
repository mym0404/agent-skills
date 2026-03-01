---
name: electron-skills
description: >
  Electron best practices for secure preload boundaries and safe IPC patterns.
  Use when building or refactoring Electron main/preload/renderer code.
license: MIT
metadata:
  author: mj
  version: "1.0.0"
---

# Electron Skills

Compact guidance for safe Electron architecture.

## Rule Categories by Priority

| Priority | Category          | Impact   | Prefix      |
| -------- | ----------------- | -------- | ----------- |
| 1        | Security Boundary | CRITICAL | `security-` |

## Quick Reference

- `security-minimal-preload-api` - Expose only narrow APIs via preload

## How to Use

Read the rule file for examples:

```
rules/security-minimal-preload-api.md
```

## Full Compiled Document

For expanded guidance: `AGENTS.md`
