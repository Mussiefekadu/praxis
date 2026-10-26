---
name: init
description: >
  Install Praxis's per-project files (rules hub, phase INDEX + template,
  README/.gitignore/verify seeds) into the current repo. Safe to re-run: never
  overwrites existing files. Manual only — /praxis:init.
disable-model-invocation: true
allowed-tools: Bash, Read, Glob
---

# /praxis:init — Install project files

Skills and subagents ship with the plugin; this copies the files that belong
to **the project**. `${CLAUDE_PLUGIN_ROOT}/templates` is the source.

## Procedure

1. Confirm the cwd is the project root (a git repo, or ask).
2. Copy **without overwriting** (skip and report anything that exists):

   | From `${CLAUDE_PLUGIN_ROOT}/templates/` | To |
   | --------------------------------------- | -- |
   | `rules/*`                               | `.claude/rules/` |
   | `phases/INDEX.md`                       | `planning/phases/INDEX.md` |
   | `phases/TXX-template.md`                | `planning/phases/_TEMPLATE.md` |
   | `seeds/readme/README-SEED.md`           | `planning/README-SEED.md` |
   | `seeds/gitignore/gitignore-SEED`        | `planning/gitignore-SEED` |
   | `seeds/verify/*`                        | `planning/verify-SEED/` |

   Use `cp -n -R` (Git Bash / macOS / Linux) or per-file `Test-Path` checks on
   plain PowerShell.
3. Report what was copied vs skipped.

**Next-step button (mandatory):** do not tell the user to type the next
command. End the turn with `mcp__ccd_turn__wrap_up` — `recap` = files copied
and skipped; `ask` = "Start with the grill?"; `actions[0]` = reply label
"Grill the idea", message `/praxis:grill-me`; second reply "Skip to
bootstrap", message `/praxis:bootstrap`. If `wrap_up` is
unavailable, print the commands instead.

## Do not

- Overwrite existing files
- Install skills or agents into the project (the plugin provides them)
- Implement product code
