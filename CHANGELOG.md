# Changelog

Versions match `version` in `.claude-plugin/plugin.json`. Installed copies stay
on their version until it changes, so every release that changes skill
behavior bumps it.

## 0.5.0 — 2026-10-27

- **Plan review:** `/praxis:plan` now has a fresh `plan-reviewer` subagent read
  the task file cold and flag anything an executor would have to guess (missing
  paths, undefined terms, steps without a verify, uncovered acceptance criteria,
  open design decisions). A task becomes `Planned` only after a READY review;
  at most two fix-and-review rounds.
- Plans include an **AC coverage** table mapping each acceptance criterion to
  steps and tests.
- README rewritten around install → first 10 minutes → work loop → skills; the
  philosophy moved to `METHODOLOGY.md`.
- `audit-rules` checks the plugin's subagent roster instead of expecting
  `.claude/agents/` files.

## 0.4.0 — 2026-10-27

- New **`/praxis:status`**: read-only report of done / in-progress / planned /
  blocked tasks and git state, with one next-step button. Claude can run it on
  its own when you ask "where was I?".

## 0.3.0 — 2026-10-27

- **Stack-neutral verify gate.** Each project declares its verify command, lint
  and test commands, pre-commit hook and required files in the hub's Verify gate
  section; `execute` and `complete` check that instead of requiring a Makefile,
  lefthook and golangci. `bootstrap` picks the gate for your stack; Go seeds
  remain the Go starting point.
- T01 must include one trivial test so verify has something to run.

## 0.2.0 — 2026-10-26

- Packaged as the **Praxis** Claude Code plugin (formerly Turboplan): install
  with `/plugin marketplace add Mussiefekadu/praxis`. Works on Windows.
- New `/praxis:init` replaces `scripts/install-into.sh`; copies rules, phase
  templates and seeds without overwriting.
- Skills renamed and namespaced: `init`, `grill-me`, `bootstrap`,
  `add-feature`, `plan`, `execute`, `complete`, `learn`, `audit-rules`.
- Every skill ends with a next-step button instead of asking you to type the
  next command.
- Product-specific constraints live in the hub's Project constraints section;
  plugin skills are no longer edited per project.
