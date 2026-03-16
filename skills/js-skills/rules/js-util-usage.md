---
title: "@mj-studio/js-util Source-of-Truth Usage"
impact: HIGH
impactDescription: Keeps @mj-studio/js-util usage aligned with the upstream public API and best practices without duplicating local guidance
tags: js-util, mj-studio, llms, source-of-truth, utilities
---

## @mj-studio/js-util Source-of-Truth Usage

When `@mj-studio/js-util` is already in use, or when one of its public helpers is a candidate for the task, treat the upstream `llms.txt` as the single source of truth for function availability, signatures, options, usage patterns, and best practices. Do not invent local conventions and do not copy the upstream guide into this skill.

Source of truth:
- [Repository](https://github.com/mj-studio-library/js-util)
- [Original llms.txt](https://github.com/mj-studio-library/js-util/blob/master/llms.txt)
- [Raw llms.txt](https://raw.githubusercontent.com/mj-studio-library/js-util/master/llms.txt)

### Required Workflow

1. Open the upstream `llms.txt` before introducing, reviewing, or refactoring `@mj-studio/js-util` usage.
2. Confirm the exact public function name, signature, options, and documented best practice there.
3. Apply the upstream guidance in code review and implementation without duplicating the guide locally.
4. Re-check `llms.txt` when usage is uncertain or the package may have changed.

### Do Not

- Do not rely on memory for public API shape or behavior.
- Do not add local best-practice text that can drift from the upstream package guide.
- Do not paraphrase all function docs into this skill. Keep this rule as a pointer to the source of truth.

This rule applies to every public function from `@mj-studio/js-util`. If the upstream `llms.txt` and local assumptions disagree, follow the upstream document and update the calling code instead of extending this skill with copied guidance.

Reference:
[Repository](https://github.com/mj-studio-library/js-util)
[@mj-studio/js-util llms.txt](https://github.com/mj-studio-library/js-util/blob/master/llms.txt)
