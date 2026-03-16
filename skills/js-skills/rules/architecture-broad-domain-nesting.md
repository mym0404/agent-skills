---
title: Organize Code by Feature and Keep Shared Pieces in feature/common
impact: HIGH
impactDescription: Keeps feature ownership clear by grouping code by feature, keeping shared code in feature/common, and nesting submodules under the owning feature
tags: typescript, javascript, architecture, domain, folder, feature
---

## Organize Code by Feature and Keep Shared Pieces in feature/common

When a codebase is organized by folders, prefer feature-first structure. Top-level feature folders should represent real product/application features, shared cross-feature code should live under `feature/common`, and submodules should stay inside the feature that owns them instead of becoming new sibling roots.

**Incorrect:**

```text
src/feature/campaigns/
src/feature/campaign-logs/
src/feature/campaign-results/
src/feature/hooks/
src/feature/utils/
src/feature/components/
```

Why it fails: Related code gets split by technical type instead of ownership. Shared and feature-specific pieces are mixed together, and parent-owned areas such as `campaign-logs` or `campaign-results` drift into fake top-level features.

**Correct:**

```text
src/
  feature/
    common/
      hooks/
      providers/
      utils/
      ui/
        components/
    campaigns/
      hooks/
      providers/
      repositories/
      logic/
      ui/
        components/
        pages/
      logs/
      results/
    accounts/
      hooks/
      providers/
      repositories/
      logic/
      ui/
        pages/
```

Feature folders may contain only the subfolders they actually need. Common examples are:

- `utils/`
- `hooks/`
- `providers/`
- `repositories/`
- `logic/`
- `ui/components/`
- `ui/pages/`

Apply this rule as follows:

- Split primary application code by feature ownership first.
- Put cross-feature reusable code in `feature/common/`.
- Keep parent-owned submodules such as `logs`, `results`, `forms`, or `templates` under the owning feature.
- Do not create new top-level feature folders just because a nested area has multiple files.

Use this pattern when:

- a module belongs clearly to one feature
- some helpers are reused widely enough to justify `feature/common/`
- a nested area shares lifecycle, ownership, or vocabulary with its parent feature

Reference:
[Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
