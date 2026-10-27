---
name: status
description: >
  Read-only progress report for a Praxis project: which phase tasks are done,
  in progress, planned, blocked or pending, plus git state, then a next-step
  button for the right command. Use when the user asks "where was I?",
  "what's next?", "status", or returns to a project. /praxis:status.
allowed-tools: Bash, Read, Grep, Glob
---

# /praxis:status — Where am I, and what's next?

Report project state and offer **one** next command. **Read-only:** never edit
files, change task status, commit, or switch branches.

## Procedure

### 1. Is Praxis set up?

| Finding | Next command |
| ------- | ------------ |
| No `.claude/rules/general.md` or no `planning/phases/INDEX.md` | `/praxis:init` |
| INDEX or hub still has `{{placeholders}}` (not bootstrapped) | `/praxis:bootstrap` |

If either applies, report it and skip to step 5.

### 2. Read the INDEX

Parse the table in `planning/phases/INDEX.md`: ID, Title, Status, Depends-on.
Count tasks by status: `✅`, `InProgress`, `Planned`, `Blocked`, `Pending`.
A dependency is met when its row is `✅` (or `—`).

For an `InProgress` or `Blocked` task, open its task file and read:
- whether **Verification** is filled and Acceptance criteria are all checked
- the latest **Status History** row (date + note — the blocker reason, if any)

### 3. Git state

```bash
git branch --show-current
git status --short | wc -l
git log -1 --format='%h %s (%cr)'
```

Note when the current branch is not the active task's stub-stem branch, and
when there are uncommitted changes.

### 4. Pick the next command (first match wins)

| State | Next command | Button label |
| ----- | ------------ | ------------ |
| `InProgress` task, Verification filled and AC all checked | `/praxis:complete TXX` | Complete TXX |
| `InProgress` task, otherwise | `/praxis:execute TXX` | Resume TXX |
| `Blocked` task | `/praxis:plan TXX` | Re-plan TXX |
| `Planned` task | `/praxis:execute TXX` | Execute TXX |
| First `Pending` task with all dependencies met | `/praxis:plan TXX` | Plan TXX |
| Every task `✅` | `/praxis:add-feature` | Add a feature |
| `Pending` tasks remain but none have dependencies met | none — report the dependency problem |

More than one `InProgress` task breaks the one-at-a-time rule: list them and
let the user pick instead of choosing.

### 5. Report

```
## Praxis status — <product>
Progress: N/M ✅ · InProgress: TXX · Planned: … · Blocked: … · Pending: …
### Active
- TXX <title> — <status>, last update <date>: <note>
### Blocked
- TYY <title> — <reason> / none
### Git
- Branch: <branch> (expected <stub-stem>) · Uncommitted: N files · Last commit: …
### Next
<command> — <one-line why>
```

Keep it short: list only active, blocked and the next pending task, not every row.

**Next-step button (mandatory):** do not tell the user to type the next
command. End the turn with `mcp__ccd_turn__wrap_up` — `recap` = progress line
plus anything wrong (blocked task, wrong branch, uncommitted work); `ask` = one
short question; `actions[0]` = a reply with the label and message from step 4.
If `wrap_up` is unavailable (not the Claude desktop app), print the command on
its own line instead.

## Do not

- Edit any file, change any status, commit, push, or switch branches
- Run the verify command or tests (that is execute/complete's job)
- Offer more than one next command, unless the user must choose between
  several InProgress tasks
