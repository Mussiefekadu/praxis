---
paths:
  - "{{GLOB_1}}"
  - "{{GLOB_2}}"
---

# {{DOMAIN_NAME}} Rules

<!-- BOOTSTRAP: rename this file to a real spoke (e.g. api-client.md). Fill tables from real failures and docs. -->

<!-- Claude Code's rules mechanism has no `description` frontmatter field — the
     H1 above plus Scope below are what explains this spoke to a reader. If this
     domain genuinely has no natural file-glob shape (rare — most do), delete the
     `paths:` block entirely; the file then always loads, like the hub. Keep any
     such unscoped spoke short, since it adds fixed cost to every session. -->

## Scope

What this spoke covers:

- {{SCOPE_1}}
- {{SCOPE_2}}

What this spoke does **not** cover (route elsewhere):

- {{OUT_OF_SCOPE}}

## Invariants

1. {{INVARIANT_1}}
2. {{INVARIANT_2}}

## External docs

**Docs (if stuck):** {{CANONICAL_DOC_URL}}

- Prefer official docs over blog posts. Date-stamp when consulted.
- last consulted: {{YYYY-MM}}

## Problem classes

### `{{Problem Class Name}}`

| Symptom | Cause | Fix |
| ------- | ----- | --- |
| {{observable}} | {{root pattern}} | {{actionable steps}} |

<!-- last-verified: YYYY-MM -->

### `{{Another Class}}`

| Symptom | Cause | Fix |
| ------- | ----- | --- |
| … | … | … |

<!-- last-verified: YYYY-MM -->
