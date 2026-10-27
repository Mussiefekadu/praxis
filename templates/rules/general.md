# {{PRODUCT_NAME}} — Agent Rules

This file is the always-on entry for Claude Code (`.claude/rules/general.md`).
It has no `paths:` frontmatter, so Claude Code loads it at the start of every
session. Domain playbooks live beside it as `.claude/rules/*.md` — most carry
`paths:` frontmatter so Claude Code loads them automatically when a matching
file is read or edited (Claude Code's native path-scoped rules mechanism).
Invocable workflows ship with the Praxis plugin as `/praxis:*` skills.

**`.claude/rules/` is the sole source of truth for rules.** Do not recreate a
parallel rules tree elsewhere.

**Hub → spoke (hard):** this file is the hub. Domain playbooks are spokes.
Do not collapse provider/API/framework detail into this file — route via the
tables below. New spokes must be added to the Routing Map and/or Problem Class table.

<!-- BOOTSTRAP: replace all {{PLACEHOLDERS}}. Rewrite Architecture, Routing, Safety, and Skills to match this project. -->

---

# Karpathy Behavioral Guidelines

These guidelines apply **at all times** when coding in an agentic environment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:

- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- If you write 200 lines and it could be 50, rewrite it.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- Remove imports/variables/functions that YOUR changes made unused.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

For multi-step tasks, state a brief plan with verification steps:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
```

Name the **capability layer** you are advancing so work stays ordered.

---

## CRITICAL: Read Rules Before You Act

1. **Path-scoped spokes auto-load.** A domain spoke with `paths:` frontmatter
   is loaded automatically the moment you read or edit a matching file — you
   don't have to fetch it by hand. Still, explicitly read the matching rule
   file(s) in `.claude/rules/` before proposing a change to a domain, especially
   when creating a brand-new file that won't trigger auto-load on its own.
2. **This file always applies** — it carries no `paths:` frontmatter, so
   Claude Code loads it at the start of every session.
3. **Rules override defaults.** If a rule conflicts with your instincts, the
   rule wins.
4. **If no rule covers the situation, stop and ask** rather than guessing.

### Routing Map — Which Domain Rule To Read

| When you are working on... | Read and follow |
| -------------------------- | --------------- |
| {{DOMAIN_A_DESCRIPTION}} | `.claude/rules/{{domain_a}}.md` |
| {{DOMAIN_B_DESCRIPTION}} | `.claude/rules/{{domain_b}}.md` |
| Shared language craft, tests, lint | `.claude/rules/{{lang}}.md` |
| Anything else or unsure | this file + ask |

When a task spans multiple areas, read every applicable rule file before starting.

---

## Project Architecture

- **Product**: {{PRODUCT_NAME}}
- **Shape**: {{SHAPE}}
- **Platforms**: {{OS_LIST}}
- **Primary language**: {{LANGUAGE_VERSION}}
- **Build / verify**: `{{VERIFY_COMMAND}}` (see [Verify gate](#verify-gate))

```
{{REPO_TREE_SKETCH}}
```

{{DOMAIN_SUBSYSTEMS}}

### Inspiration sources

{{INSPIRATION_SOURCES}}

### Build & Run

```bash
{{BUILD_RUN_SNIPPET}}
```

### Verify gate

<!-- BOOTSTRAP: fill every field for this stack. /praxis:execute and /praxis:complete read this section and stop if it is missing or still has {{placeholders}}. -->

| Field | Value |
| ----- | ----- |
| **Verify command** | `{{VERIFY_COMMAND}}` — one command that runs lint **and** test (+ build when the stack builds) |
| Lint | `{{LINT_COMMAND}}` (config: `{{LINT_CONFIG}}`) |
| Test | `{{TEST_COMMAND}}` |
| Build | `{{BUILD_COMMAND}}` \| n/a |
| Pre-commit hook | `{{HOOK_TOOL}}` (config: `{{HOOK_CONFIG}}`) runs the verify command \| `none` (human-approved) |
| Required files | {{VERIFY_FILES}} — each must exist at the repo root |

Package tests alone are never the gate. Typical shapes (pick one, or the stack's own equivalent):

| Stack | Verify command | Lint / test | Hook |
| ----- | -------------- | ----------- | ---- |
| Go | `make verify` | golangci-lint / `go test ./...` | lefthook |
| Node / TS | `npm run verify` | eslint (+ `tsc --noEmit`) / vitest or jest | husky or lefthook |
| Python | `make verify` or `nox -s verify` | ruff (+ mypy) / pytest | pre-commit |
| Rust | `just verify` or `make verify` | `cargo clippy -- -D warnings` / `cargo test` | lefthook or pre-commit |

---

## Delivery Principles

**MVP sequence of record:** [`planning/phases/INDEX.md`](../../planning/phases/INDEX.md).
Use `/praxis:plan` → `/praxis:execute` → `/praxis:complete`. Do not skip layers.

### Layered build order

See [`planning/phases/INDEX.md`](../../planning/phases/INDEX.md) for the
current layered build order and task sequence.

Wire features in dependency order. **Stop at a failed verification.** Do not
advance to the next layer on a broken base.

### Atomic work + verification gates

Prefer small, independently checkable units. Before calling a chunk done, run
the cheapest proof that fits (package tests, manual path), and the full
`{{VERIFY_COMMAND}}` before a task is called done.

### Model split: expensive plan → cheap execute

Use a **large / expensive** model for `/praxis:plan` (and bootstrap / hard
dialectic), and a **smaller / faster / cheaper** model for `/praxis:execute`
and usually `/praxis:complete`.

**Plan handoff bar:** `/praxis:plan` must write an Execution plan detailed
enough that a **lesser agent** can implement it without rediscovering
architecture, guessing file paths, or inventing design. Prefer:

- Concrete files/packages to create or edit
- Function/type/API shapes or signatures when non-obvious
- Ordered steps with `→ verify:` checks
- Explicit test cases to add
- Exact verify/lint commands
- Risks, pitfalls, and "do not" edges

If the plan is only high-level ("wire the handler", "add tests"), it is
**incomplete** — deepen it before marking it planned.

---

## Safety / Workflow Rails

NEVER do these without explicit user approval:

- Modify `.claude/rules/` files (except via `/praxis:learn`)
- Commit or push to git **outside** `/praxis:complete`
- Force-push, rewrite shared history, or push secrets
- {{EXTRA_NO_GO_1}}
- {{EXTRA_NO_GO_2}}

### Package managers / toolchain

{{TOOLCHAIN_RULES}}

### Verification defaults

- Prefer `{{VERIFY_COMMAND}}` after substantive edits
- If verification fails, fix that layer — do not advance

---

## Rule Maintenance (Self-Evolving Rules)

<!-- last-verified: 2026-08 -->

Rules must improve over time. Follow the dialectic of cognition:

> *From the particular to the general, then from the general to the particular.*

**Operational harness:** invoke `/praxis:learn` (see
plugin skill `learn`).

**Write only to** `.claude/rules/*.md`.

**Trigger:** Apply this process when ANY of these are true:

- Debugging took >5 minutes
- External documentation was consulted
- More than one corrective attempt was needed before the fix worked
- The root cause was non-obvious from the error message alone

Trivial fixes (typos, config values, one-line renames) should **NOT** trigger
rule updates — they add noise without improving recognition.

### 0. Abort gate — is this generalizable?

Before extracting a principle, apply the one-sentence test: **can you state the
rule without naming a specific file, function, class, variable, or endpoint?**
If not, stop here — there is nothing to encode. The value stays in the diff.

### 1. Particular → General (abstract)

Extract the **class** of problem, not the instance.

### 2. General → Particular (encode)

Write into the appropriate `.claude/rules/*.md`. A good entry answers:

1. **What does it look like?** (symptoms)
2. **What causes it?** (root pattern)
3. **How do you fix it?** (actionable steps)

```
### `<Problem Class Name>`

| Symptom | Cause | Fix |
| ------- | ----- | --- |
| `<observable>` | `<root pattern>` | `<actionable steps>` |

<!-- last-verified: YYYY-MM -->
```

### 3. Verify — practice tests theory

Would a cold-read AI recognize the symptom and apply the fix without
re-investigation? If not, refine.

### 4. Check for contradictions

When a new principle conflicts with an old one for the same symptoms, encode
the boundary condition — don't silently overwrite.

### 5. Deduplicate before adding — prefer refinement over proliferation

### 6. Rule decay — keep the knowledge base alive

Add `<!-- last-verified: YYYY-MM -->`. Review entries older than 6 months when
working in that domain.

### 7. File size threshold — split before rules become unreadable

If a `.claude/rules/*.md` file exceeds ~550 lines after an edit, propose a
split to the user. Never split without approval.

### 8. Path scoping — keep spokes auto-loading correctly

When creating or editing a spoke, keep its `paths:` frontmatter matched to the
files it actually governs. A spoke with no `paths:` frontmatter loads on
**every** session — reserve that only for genuinely cross-cutting conventions
with no natural glob shape, and keep those short. Prefer `paths:` scoping
wherever the domain maps to real file patterns.

---

## Routing Table — Problem Class → Rule File

| Change type / Problem domain | Rule file |
| ---------------------------- | --------- |
| {{DOMAIN_A}} | `.claude/rules/{{domain_a}}.md` |
| {{DOMAIN_B}} | `.claude/rules/{{domain_b}}.md` |
| Language / tests / lint | `.claude/rules/{{lang}}.md` |
| Cross-cutting / unknown | `.claude/rules/general.md` |

---

## Subagent Roster

Delegate facts-gathering and mechanical work to the subagents defined in
`.claude/agents/*.md` instead of doing it serially yourself. Invoke via the
`Agent` tool with the subagent's name, e.g. `Agent(explorer)`. Keep decisions,
design, and human interaction on the parent.

| Subagent | Purpose |
| -------- | ------- |
| `explorer` | Find files, read code, trace calls, map architecture, verify facts |
| `web-researcher` | Fetch docs, check API references |
| `implementer` | Well-scoped feature/bugfix from a clear spec |
| `refactorer` | Extract/rename/move — behavior-preserving changes |
| `test-runner` | Run tests, diagnose failures |
| `code-reviewer` | Correctness/lint/style/bug review of a diff |
| `verifier` | Skeptical independent check that claimed work is actually done |
| `plan-reviewer` | Cold-read check that a plan needs no guessing |
| `doc-writer` | Docs/changelog/README updates from diffs |

See [`METHODOLOGY.md` — Subagent delegation](../../METHODOLOGY.md#4--subagent-delegation)
for delegation rules (parallelize independent work, escalate on bad output, etc.).

---

## Skills Inventory

| Skill | Path | Purpose |
| ----- | ---- | ------- |
| `/praxis:bootstrap` | plugin skill `bootstrap` | From goal → rules + phases + dep spokes + README + .gitignore + root verify gate |
| `/praxis:plan` | plugin skill `plan` | Plan one phase task |
| `/praxis:execute` | plugin skill `execute` | Implement one phase task |
| `/praxis:complete` | plugin skill `complete` | Verify + dialectic + INDEX ✅ + commit + push (default) + manual test + next branch |
| `/praxis:learn` | plugin skill `learn` | Encode session learnings into rules |
| `/praxis:audit-rules` | plugin skill `audit-rules` | Read-only audit of rules/skills vs tree |
| `/praxis:status` | plugin skill `status` | Read-only progress report + next-step button |
| `/praxis:grill-me` | plugin skill `grill-me` | Interrogate an idea over a design tree until shared understanding; feeds /praxis:add-feature |
| `/praxis:add-feature` | plugin skill `add-feature` | New feature → context → phase stubs appended to INDEX |

## Project constraints

<!-- BOOTSTRAP: product-specific hard constraints that /praxis:plan, execute and complete must obey (stack, verify command, forbidden trees, no-gos). -->
