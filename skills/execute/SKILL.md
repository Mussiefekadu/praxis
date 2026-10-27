---
name: execute
description: >
  Execute one Planned/Pending MVP phase task from planning/phases/. Implement
  until acceptance criteria pass, record verification, then hand off to
  /praxis:complete. Manual only — /praxis:execute TXX.
disable-model-invocation: true
allowed-tools: Bash, Read, Grep, Glob, Edit, Write, Agent(praxis:implementer, praxis:refactorer, praxis:code-reviewer, praxis:test-runner, praxis:verifier)
---

# /praxis:execute — Execute one atomic MVP task

You implement **exactly one** task from `planning/phases/`. Stop when Acceptance
criteria pass (or blocked). Closing the task is **`/praxis:complete`**.

This skill is intended to run on a **medium or small** model when the
Execution plan from `/praxis:plan` meets the hub handoff bar. Default to
small when the plan is thorough; use medium for non-trivial implementations
the plan flags as complex. Follow the plan
literally; if the plan is too vague to proceed, **stop** and send back to
`/praxis:plan` (or ask the user to re-plan on a large model) — do not invent a
new design. If the plan marks **`Execute model recommendation: large`**, tell
the user before heavy work if they are still on a small model.

> 💡 Model sizing is a **recommendation**, not a hard constraint — run this on
> whatever model you have available.  

## Arguments

- Task id: `T01` … `Tnn`  
- If omitted: first INDEX `Planned`, else first actionable `Pending`

## Hard constraints

Also obey the **Project constraints** section of `.claude/rules/general.md`.

1. Obey **Karpathy Behavioral Guidelines** in `.claude/rules/general.md` in full
   (Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution).
   Do not treat "surgical" as the only rule.  
2. Read matching domain spokes before editing those areas.  
3. **New code requires tests** in this task, in the stack's idiomatic style
   (e.g. table-driven in Go, parametrized in pytest).  
4. **Run the project's verify command** (hub → **Verify gate**) before claiming
   AC passed. Auto-fix first; fail only on remaining unfixable issues.  
5. **Verify tooling presence (hard abort):** run the presence check in step 3
   below. If anything is missing: **stop**, Status `Blocked`, and tell the user to
   restore verify tooling — do **not** treat unit tests alone as "lint+test green."  
6. Never vendor forbidden reference trees.  
7. Do not commit unless the user explicitly asked (complete commits + pushes on close-out).  
8. Only one phase task InProgress unless human approved parallel work.

## Procedure

### 0. Preconditions

- Read task + INDEX  
- Depends-on is `✅` / `Done` / `—`  
- If no Execution plan section, run `/praxis:plan` first  

### 1. Status → `InProgress` (task file + INDEX)

Log Status History.

### 2. Implement

- Follow Execution plan  
- Add/update tests with the code  
- Auto-fix / lint continuously when practical  
- **Delegate bounded subtasks** when they are well-scoped and mechanical
  (behavior-preserving refactors, isolated feature units with a clear spec) to
  `Agent(implementer)` / `Agent(refactorer)`; keep design decisions and
  cross-file coordination on the parent  
- If you introduce a new library, add/update its `.claude/rules/<name>.md` spoke
  (Docs URL) and a README Dependencies & docs row when practical — otherwise note
  it for `/praxis:complete`  

### 3. Verify gate (mandatory)

**Presence check first** (fail closed). Read the **Verify gate** section of
`.claude/rules/general.md`, then:

1. If the section is missing or still has `{{…}}` placeholders → stop.
2. Every path under **Required files** exists (`test -f <path>` for each).
3. The **Verify command** runs lint **and** test — not tests alone.
4. If a pre-commit hook is declared, its config file exists and invokes the
   verify command. `none` is acceptable only when the hub says the human approved it.
5. Run the verify command.

If any check fails: stop — Status `Blocked`; do **not** hand off as passed with
only package tests. Record the verify command and its outcome in
**Verification**. If verify fails: fix or set Status `Blocked`.

**Post-edit checks (when subagents are available):** after non-trivial edits,
launch `Agent(code-reviewer)`, `Agent(test-runner)`, and `Agent(verifier)`
**in the background** and continue; fold their findings in before claiming AC
passed. A reviewer-found bug blocks the handoff until fixed.

### 4. Acceptance → all checked or Status `Blocked`

AC must include: tests present for new code; lint+test suite green.

### 5. Record on the task file

Fill Verification and Files Modified. Leave INDEX as `InProgress` until
`/praxis:complete`.

### 6. Hand off (mandatory)

Summarize the result, then give the user the next-step button. `/praxis:complete` will re-verify → dialectic → INDEX → ✅ → commit → push (default; `--no-push` to skip) → Manual test handoff → next branch.

**Next-step button (mandatory):** do not tell the user to type the next command. End the turn with `mcp__ccd_turn__wrap_up` — `recap` = what is now true; `ask` = one short question; `actions[0]` = a reply with `label` "Complete TXX" and `message` `/praxis:complete TXX` (if Blocked, offer "Re-plan TXX" → `/praxis:plan TXX` instead)
. If `wrap_up` is unavailable (not the Claude desktop app), print the command on its own line instead.
If the user already asked to execute **and** complete in one go, run
`/praxis:complete` yourself now.

### 7. Output format

```
## Executed — TXX Title
### Result
Acceptance passed / Blocked
### Verification
- lint: …
- tests: …
- …
### Files touched
- …
### Next
button → /praxis:complete TXX
```

## Do not

- Execute two phase tasks in one invocation  
- Mark INDEX `✅` without `/praxis:complete`  
- Write `Done` into the INDEX Status column  
- Skip tests or skip the lint+test verify gate  
- Treat package tests alone as full verify when the hub's Verify gate is missing or its required files are absent  
