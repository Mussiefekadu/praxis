---
name: bootstrap
description: >
  From a written product goal, adapt .claude/rules, seed
  planning/phases, create dependency rule spokes from
  provided docs, write a human README.md, and an appropriate .gitignore.
  Manual only — /praxis:bootstrap. Does not implement product features.
disable-model-invocation: true
allowed-tools: Bash, Read, Grep, Glob, Edit, Write, WebFetch, WebSearch, Agent(praxis:explorer, praxis:web-researcher)
---

# /praxis:bootstrap — Goal → rules + phases + skills + human README

You install or retarget **Praxis** for this repository. You produce agent
operating infrastructure **and** a human-facing `README.md`. You **do not**
implement product features.

**Two audiences (keep in sync as the project grows):**

| Audience | Primary artifacts |
| -------- | ----------------- |
| **LLMs / agents** | `.claude/rules/*.md` (skills and subagents ship with the plugin) |
| **Humans** | Root `README.md` (+ phase INDEX for operators) |

## Arguments (user provides in the invocation)

- **Goal** — what users get when done (1–3 paragraphs, end-user perspective)
- **Technical description** — language, runtime, OS, packaging, architecture
- **Non-goals** — explicit exclusions
- **Constraints** — secrets policy, no-gos, verify command, toolchain preferences
- **Dependencies / libraries** — frameworks the product will use (e.g. Cobra,
  SQLite driver, Astro) — each should become a rules spoke
- **References / docs** — paths/URLs to study (official docs, OpenAPI, examples;
  reimplement, do not vendor)
- Optional: preferred layer list / task count target

**CRITICAL — context gathering is mandatory.** If the user invokes `/praxis:bootstrap`
without a detailed goal and technical description, you MUST ask for them before
proceeding. Do not invent the goal. Do not guess the stack. Ask:

1. What does the user get when the project is done? (end-user perspective)
2. What is the technical scope? (language, runtime, OS, packaging, architecture)
3. What is explicitly out of scope?
4. What dependencies or external APIs will it use?
5. Are there reference implementations to study?

If the answer is vague, ask follow-ups until you have enough to produce an accurate
hub + spokes + phase plan. A human who won't answer these questions isn't ready to
bootstrap — stop and say so rather than producing a wrong architecture.

**The user may provide this context in the initial prompt, in follow-up answers,
or as a file attached to the chat session.** If a file is attached and clearly
contains the project specification (PRD, design doc, notes), read it and
extract the answers — do not re-ask for information already provided.

## Hard constraints

1. Write rules only under `.claude/rules/`.
2. Delete spokes/skills that cannot apply; replace wrong-provider files (do not light-edit).
3. Seed `planning/phases/INDEX.md` + one stub file per INDEX row.  
4. Skills are plugin-provided and not edited per project. Put product-specific constraints in a **Project constraints** section of `.claude/rules/general.md`; the plan/execute/complete skills read it.  
5. If `.claude/rules/general.md` or `planning/phases/INDEX.md` is missing, run `/praxis:init` first.  
6. Do not commit unless the user explicitly asks.  
7. Do not implement product features. Specifically:
   - NEVER create application source files or source directories (e.g. `cmd/`,
     `internal/`, `src/`, `migrations/`). These belong to T01 via `/praxis:execute`.
   - NEVER initialize the app's module/package (`go mod init`, `npm init`,
     `cargo new`, `uv init`, …). T01 does that.
   - In-scope (NOT product code): verify gate files (task runner, lint config,
     hook config), .gitignore, README.md, .claude/rules/*.md,
     planning/phases/INDEX.md + stubs.  
8. Bootstrap AC requires **git repo + verify gate files at repo root + hook
   installed + hub Verify gate filled** (see step 5). The gate is chosen for the
   user's stack; the Go seeds in `planning/verify-SEED/` are one example, not a
   requirement for other stacks. Seeds under `planning/` alone are **not** enough.
   **Do not require the verify command to pass yet** — there is no source code.
   T01 creates the skeleton; verify passes for the first time in T01.  
9. Use the **stack the user specified** in the technical description. Do not
   silently substitute languages, frameworks, or toolchains. If the user
   specified Go, use Go. If they specified React, use React. Raise any
   non-obvious choices as **bootstrap concerns** with rationale.  
10. Use the **toolchain versions the user specified** (or current environment
    baseline). Do not silently upgrade. If the environment constrains versions,
    document under **Bootstrap concerns**.  
11. For every named dependency / provided doc set that agents will use: create a
    **dedicated spoke** under `.claude/rules/` with an **External docs** link to
    the official source, and mirror a short human summary in `README.md`.  
12. Create or replace root **`README.md`** with: emoji section headers; ASCII
    banner; a brief human **Summary**; a **Table of Contents** immediately under
    the summary; then the body sections — humans read this; agents read rules/skills.  
13. Create or update root **`.gitignore`** appropriate to the stack — use best
    judgment (always secrets + scratch; language/tooling artifacts as needed).  
14. `.claude/agents/*.md` is generic subagent infrastructure (explorer,
    web-researcher, implementer, refactorer, test-runner, code-reviewer,
    verifier, doc-writer) — (plugin-provided) — never copy or product-adapt it. Leave the
    shipped roster intact; only add a new subagent file if the product
    genuinely needs a role none of these cover.  

## Procedure

### 1. Inventory current agent files

List `.claude/rules/*.md`. Classify each:
DELETE / ADAPT / KEEP (per Praxis Guide 01).

**Delegate:** fan out explorer subagents (one per independent area of the
codebase/tree) to build the inventory and assess each file; delegate doc
fetches for dependency spokes to web-researcher subagents in parallel. Keep
classification **decisions** on the parent.

### 2. Rewrite hub

Update `.claude/rules/general.md`:

- **Strip every reference to Praxis** — this file is about THIS product, not
  the methodology that bootstrapped it. No "Praxis hub", no "Praxis
  project", no "bootstrap" in the safety rails exception list.
- **Product name, architecture, build/verify, safety no-gos** — all project-specific
- **Routing Map + Problem Class table** for new spokes (include every dep spoke)
- **Skills inventory** — list the plugin's skills (`/praxis:*`)
- **Layered delivery**: reference `planning/phases/INDEX.md` only. Do NOT copy
  task IDs, layer tables, or phase details into the hub.
- **Dialectic examples** from **this** domain (failure-mode illustrations only)
- **Do NOT include** generic language-preference evangelism ("Prefer Go",
  "Astro vs Vue vs Wails"), generic test/lint/git philosophy, or toolchain
  upgrade policies that match the template defaults. These are methodology
  opinions, not project ground truth. If the user specified a language,
  framework, or toolchain, state it as a fact ("Uses Go 1.26+") not as a
  preference ("Prefer Go because…").  

### 3. Spokes (domains + dependency docs)

- Delete obsolete  
- Create/adapt **product domain** spokes with invariants + at least one symptom table skeleton  
- Language craft spoke if applicable (`go.md`, etc.)  
- **Dependency / docs spokes (mandatory when deps or docs are provided):**

  For each library, framework, CLI toolkit, or external API the Goal/Constraints/
  Dependencies/References name (e.g. Cobra, Moonshot API, Cloudflare):

  1. Prefer **official documentation** (fetch or use provided URLs).  
  2. Create `.claude/rules/<name>.md` (e.g. `cobra.md`) including:
     - Title + when to apply (`paths:` frontmatter globs if the domain has a natural file-pattern shape)  
     - **`Docs (if stuck):`** canonical URL near the top (e.g. https://cobra.dev/docs/)  
     - Scope / invariants / layout or API conventions for **this** product  
     - At least one symptom / cause / fix table skeleton (or real patterns from docs)  
     - `<!-- last-verified: YYYY-MM -->`  
  3. Add the spoke to the hub **Routing Map** and Problem Class table.  
  4. Summarize the same dependency for humans in `README.md` → **Dependencies & docs**
     (name, role, docs URL, link to the `.md` spoke).  

  Use `templates/rules/EXAMPLE-domain.md` as the shape. Delete `EXAMPLE-domain.md`
  when real spokes exist.

### 4. Skills

Ensure these exist and hard constraints match the product:

- `grill-me`, `plan`, `execute`, `complete`, `learn`, `audit-rules`, `add-feature`  
- Keep `bootstrap` for future retargets  

Remove skills that only serve deleted stacks.

### 4b. Clean up init leftovers

After verify gate files are adapted to root, delete these stale seeds:

```bash
rm -f planning/_TEMPLATE.md
rm -rf planning/verify-SEED
rm -f planning/README-SEED.md
rm -f planning/gitignore-SEED
```

### 5. Git repo + verify gate (bootstrap acceptance — mandatory)

1. If `.git` is missing: `git init` in the project root (empty repo / first commit
   later is fine). If `.git` already exists, leave it — do not destroy history.  
2. **Choose the verify gate for this stack** and ship it at project root. Use the
   stack's native tooling; a `Makefile` is optional outside Go. It must provide:
   - **One verify command** that runs lint **and** test (+ build when the stack
     builds) — e.g. `make verify`, `npm run verify`, `nox -s verify`, `just verify`.  
   - **Lint config** for the stack's linter (e.g. `.golangci.yml`, `eslint.config.js`,
     `ruff.toml` / `[tool.ruff]`, clippy settings).  
   - **A pre-commit hook** that runs the verify command (lefthook, husky,
     pre-commit, …). Only if the human explicitly declines a hook, record `none`
     and that approval in the hub.  
   Use the hub's "Typical shapes" table as the starting point; raise anything
   unusual as a **Bootstrap concern**.  
3. **Fill the hub's Verify gate section** in `.claude/rules/general.md`: verify,
   lint, test, build commands, hook tool + config, and **Required files** (every
   gate file execute/complete must find). No `{{placeholders}}` may remain there.
   Document the verify command in hub Build & Run **and** README.  
4. **Go projects (Makefile shape):** start from `planning/verify-SEED/Makefile`,
   `lefthook.yml` and `golangci.yml` (→ root `.golangci.yml`)
   and keep (or equivalent) targets:
   - `lint` — golangci-lint  
   - `test` — `go test ./...`  
   - `build` — host `go build ./...`  
   - `build-all` — multi-platform cross-compile into `dist/` (linux/darwin ×
     amd64/arm64 by default; override `BINARY` / `MAIN_PKG` / `GOOS_LIST`)  
   - `verify` — `lint` + `test` + `build` (this is the gate; not `build-all`)  
   - `fmt`, `vet`, `clean`, `install-hooks`, `help`  
   Adapt package paths (`MAIN_PKG`) to the real `cmd/` layout. For other stacks,
   delete the Go seeds instead of copying them.  
5. **Install the hook** in this clone (`lefthook install`, `npx husky init`,
   `pre-commit install`, …).  
6. Bootstrap AC **fails** if any required file is missing, if the verify command
   is not lint+test(+build), if the hook is not installed / does not invoke
   verify, or if the hub Verify gate still has placeholders.
   **Do not require the verify command to pass** — the repo has no source code yet.
   T01 creates the skeleton; verify first passes in T01.  
7. `/praxis:execute` and `/praxis:complete` must **hard-abort** when this tooling
   is later deleted — bootstrap must leave them unable to “pass” on tests alone.

### 6. Latest stable toolchain + packages (mandatory)

1. Install or upgrade the project's **primary language** to the **latest stable**
   release available for the host OS/arch (use the package manager the user
   already employs). Record the version in hub Build & Run + README Status.  
2. When scaffolding modules: use current stable dependency versions
   (`go get` current stables for Go, `go mod tidy`; for JS: `yarn` / `npm`
   stable versions; avoid knowingly unstable majors).  
3. Install lint/format tools at current stable (e.g. golangci-lint for Go,
   ESLint/Prettier for JS) so verify works.  
4. If policy or the machine blocks upgrades, list under **Bootstrap concerns**
   with the pinned version — do not silently stay on stale toolchains when an
   upgrade is possible.  

### 6b. `.gitignore` (mandatory)

Create or update root **`.gitignore`**. Prefer
`templates/seeds/gitignore/gitignore-SEED` / installed `planning/gitignore-SEED`
as a starting checklist, then **adapt with best judgment** for this product.

**Always include (unless the user explicitly wants otherwise):**

- Secrets / env: `.env`, `.env.*` (optionally allow `!.env.example` / `!.env.ref`)  
- Scratch: root-level `tmp/` (and similar local scratch dirs)  

**Stack-aware (enable what applies):**

- **Go:** binaries / `*.exe` / `*.test` / coverage profiles; local `bin/` or `dist/`
  if used; do not ignore source. Prefer not ignoring `vendor/` unless the project
  policy is to never commit it.  
- **Node / Astro / Vue / Wails frontend:** `node_modules/`, build outputs (`dist/`,
  `.astro/`, etc.), Yarn cache noise per project linker  
- **Python:** `__pycache__/`, `.venv/`  
- **OS / editors:** `.DS_Store`, common IDE folders if not already shared  

If `.gitignore` already exists: **merge** — add missing essential patterns; do not
wipe custom entries. Never commit real secrets to “fix” ignore gaps — fix the
ignore file instead.

### 5. Seed phases

1. Derive layered build order from the goal (Guide 02)  
2. Write `planning/phases/INDEX.md` with T01…Tnn

**T01 is always the app boilerplate bootstrap task (L0 skeleton).**
It creates the minimal runnable program so that the verify command passes:
- Initialize the module/package for the stack and add a minimal entry point
  (e.g. Go: `go mod init`, `cmd/<name>/main.go`, `internal/` stubs; Node:
  `package.json` + `src/index.ts`; Python: `pyproject.toml` + package dir).
- At least one trivial test so the test step has something to run.
- No business logic, no features — just the skeleton that builds.
- After T01, the verify command must pass (lint + test + build on the skeleton).  
3. Create each stub from the Praxis task template (Description, Requirements, AC, empty Execution plan section, Depends-on, Next, Layer)  
4. Final task should be E2E / holistic verification  
5. Early tasks should include verify wiring / sample test+lint green if scaffold exists  

### 6. Human README.md (mandatory)

Create or rewrite root **`README.md`** for humans (not a dump of agent rules).

Prefer the pack seed `templates/seeds/readme/README-SEED.md` (after install:
`planning/README-SEED.md` if the installer copied it):

1. **ASCII banner** for the product name (block letters in a fenced `text` block)  
2. **Summary** — brief human-readable overview (2–4 sentences: what / who / done)  
3. **Table of Contents** — immediately below the summary (link major sections)  
4. **Emoji section headers** for the body (problem, fix, status, layout, deps, building, security, …)  
5. Pitch details, status pointing at `planning/phases/INDEX.md`  
6. Repo layout table  
7. **Dependencies & docs** table (synced with dep spokes from §3)  
8. **Building with Praxis** section — link to
   [https://github.com/Mussiefekadu/praxis](https://github.com/Mussiefekadu/praxis)
   and show the plan → execute → complete loop; also link to INDEX  
9. Security / invariants relevant to humans  
10. Point methodology at [Praxis](https://github.com/Mussiefekadu/praxis)  

If a README already exists: adapt it to this shape — do not leave a stale fork
README that contradicts the new goal. Keep factual content that still applies.

**Ongoing:** as phases complete and architecture changes, keep README and rules
aligned (execute/complete/dialectic should update human docs when the story for
humans changed — not only `.md` files).

### 7. Self-check

- [ ] Routing Map ↔ rule files bijection  
- [ ] Git repo exists; verify command (lint+test) wired at root; lint config present;
      pre-commit hook installed and runs that verify (or `none`, human-approved)
      (note: verify may fail — no source code yet; T01 makes it pass)  
- [ ] Hub **Verify gate** section filled — no `{{placeholders}}` left  
- [ ] Primary language (and other baseline tools) at **latest stable**; deps installed at current stables (or concern documented)  
- [ ] Root `.gitignore` present (`.env`/variants, `tmp/`, stack artifacts — merged if pre-existing)  
- [ ] Every provided dependency/docs set has a spoke with **Docs (if stuck)** URL + README row  
- [ ] Root `README.md` exists with ASCII banner, **Summary**, **TOC**, emoji headers, deps section  
- [ ] No leftover deleted-stack names in rules/skills (grep)  
- [ ] Every INDEX row has a stub  
- [ ] Depends-on graph acyclic; T01 actionable  
- [ ] No references to "Praxis" in `.claude/rules/general.md` (aside from the skills inventory which lists `bootstrap` as a skill name)  
- [ ] Hub states stack choices as facts ("Uses Go 1.26+, React 19") not as methodology preferences ("Prefer Go", "Astro vs Vue vs Wails")
- [ ] "Karpathy Behavioral Guidelines" heading present in hub (not "Behavioral Guidelines")  
- [ ] No task IDs or layer tables in general.md (INDEX.md is the sole source of truth)  
- [ ] No product source code created (no `cmd/`, `internal/`, `pkg/`, `migrations/`)  
- [ ] Installer leftovers cleaned (no `_TEMPLATE.md`, `verify-SEED/`, `README-SEED.md`, `gitignore-SEED` under `planning/`)  
- [ ] `/praxis:add-feature` listed in hub skills inventory  
- [ ] T01 is the app skeleton bootstrap task (module init, entry point, one trivial test, verify passes)  

### 8. Output to user

```
## Bootstrap complete — {{PRODUCT}}

### Rules
- hub: …
- spokes: … (include dep spokes + docs URLs)
- deleted: …

### README
- path: README.md
- deps documented: …

### Gitignore
- patterns: .env* · tmp/ · <stack-specific> …

### Stack
- backend: {{BACKEND_LANG}}
- frontend: {{FRONTEND_FRAMEWORK}} | n/a
- concerns: … / none

### Git / hooks
- git: initialized | already present
- verify command: `<command>` (= lint + test + build when applicable)
- lint config: <file> | MISSING (fail)
- hook: <tool> configured + installed → pre-commit runs verify | none (human-approved)
- hub Verify gate: filled | placeholders left (fail)
- seeds: `templates/seeds/` → `planning/*-SEED*` → adapted to root
- toolchain: {{LANGUAGE}} <version> (latest stable) | pinned concern: …
- packages: latest stable baseline | concern: …

### Phases
- T01…Tnn listed
- First action: button → /praxis:plan T01

### Skills
- project constraints written to hub: …

### Review gate
Confirm architecture + layer order + README, then use the button.
```

**Next-step button (mandatory):** do not tell the user to type the next command. End the turn with `mcp__ccd_turn__wrap_up` — `recap` = what is now true; `ask` = one short question; `actions[0]` = a reply with `label` "Plan T01" and `message` `/praxis:plan T01` as the confirm gate (second reply "Change architecture"). If `wrap_up` is unavailable (not the Claude desktop app), print the command on its own line instead.

## Do not

- Start `/praxis:execute` unless the user explicitly chains it  
- Invent PBIs outside `planning/phases` unless the repo already uses another backlog and the user asks to bridge  
- Copy secrets from references into the repo  
- Skip git init / the verify command / the hook / the hub Verify gate and call bootstrap done  
- Leave verify seeds only under `planning/` without copying adapted files to project root  
- Leave any "Praxis" reference in `.claude/rules/general.md` (the skill inventory listing `bootstrap` is the only exception)
- State stack choices as methodology preferences — state them as facts  
- Skip a dep/docs spoke when the user named that dependency or provided its docs  
- Skip creating/updating human `README.md`  
- Skip creating/updating `.gitignore` (or leave secrets/tmp unignored)  
