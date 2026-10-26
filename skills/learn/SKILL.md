---
name: learn
description: >
  Capture session learnings into project rules. Manual; also from /praxis:complete.
disable-model-invocation: true
allowed-tools: Bash, Read, Grep, Glob, Edit, Write
---

# /praxis:learn — Capture session learnings into evolving rules

Perform rule maintenance per **Rule Maintenance (Self-Evolving Rules)** in
`.claude/rules/general.md` — that section is authoritative (triggers, abort
gate, steps **0–7**). This skill is the operational harness only.

**Store (this project)**: `.claude/rules/*.md` only.

Follow general.md steps 0–7 when encoding (refine, contradict-check, verify,
timestamp, size threshold **600**).

---

## Mode A — Debugging learnings

### A0 — Triage

Triggers: debugging >5 min; external docs; multiple corrective attempts; non-obvious root cause.

If none: "Mode A: no debugging triggers — skipping." → Mode B.

### A1 — Extract (Particular → General)

Symptom, root-cause **class**, resolution **pattern**.

### A2 — Route

Routing Table at the bottom of `general.md` → project spokes.

---

## Mode B — Code change → rule impact

### B0 — Summarize changes

### B1–B2 — Route and read spokes

### B3 — Abort gate

State the rule without naming a specific file/function/class/variable/endpoint?
If not, skip for project rules.

### B4 — Encode into `.claude/rules/*.md`

Prefer refine. Add `<!-- last-verified: YYYY-MM -->`.

---

## Shared integrity

Contradiction check · file size >600 → propose split · decay >6 months.

## Output format

Include Mode A, B, and project encodings table, plus Integrity.

If nothing: **"Nothing to capture — session was routine."**
