# 🚀 {{PRODUCT_NAME}}

```
{{ASCII_BANNER}}
```

## Summary

{{2–4_SENTENCES_FOR_HUMANS: what this repo is, who it is for, and what “done” looks like.
Keep jargon light. Point at Praxis only if useful.}}

Bootstrapped with **[Praxis](https://github.com/Mussiefekadu/praxis)** (agent rules + phased delivery).

## Table of Contents

- [Summary](#summary)
- [❗ The problem](#-the-problem)
- [🛠️ The fix (target)](#️-the-fix-target)
- [📊 Status](#-status)
- [📂 Repo layout](#-repo-layout)
- [📚 Dependencies & docs](#-dependencies--docs)
- [🔁 Building with Praxis](#-building-with-praxis)
- [🔒 Security / invariants](#-security--invariants)
- [📜 License / attribution](#-license--attribution)

---

## ❗ The problem

| What you try | What happens |
| ------------ | ------------ |
| {{TRY_1}} | {{HAPPENS_1}} |
| {{TRY_2}} | {{HAPPENS_2}} |

## 🛠️ The fix (target)

{{BULLETS_OR_SHORT_ARCHITECTURE}}

```text
{{SIMPLE_FLOW_DIAGRAM}}
```

## 📊 Status

| Area | State |
| ---- | ----- |
| 🧭 Agent rules (`.claude/rules/`) | Bootstrapped |
| 📋 MVP plan (`planning/phases/`) | Seeded — see INDEX |
| 🛠️ Product code | Not started (or note what exists) |

## 📂 Repo layout

| Path | For |
| ---- | --- |
| [`README.md`](README.md) | 👤 Humans (this file) |
| [`.claude/rules/`](.claude/rules/) | 📜 Conventions for coding agents |
| [`.claude/agents/`](.claude/agents/) | 🤖 Subagent roster (explorer, implementer, …) |
| Praxis plugin | 🧩 `/praxis:*` plan / execute / complete / … |
| [`planning/phases/`](planning/phases/) | 🗂️ MVP sequence of record |

<!-- BOOTSTRAP: add cmd/, internal/, packages as they appear. -->

## 📚 Dependencies & docs

Human-facing summary of major libraries/frameworks. Agents get detail in matching
`.claude/rules/*.md` spokes — keep both in sync as the project evolves.

| Dependency | Role | Docs | Agent rules |
| ---------- | ---- | ---- | ----------- |
| {{DEP_1}} | {{ROLE}} | {{DOC_URL}} | [`.claude/rules/{{spoke}}.md`](.claude/rules/{{spoke}}.md) |

## 🔁 Building with [Praxis](https://github.com/Mussiefekadu/praxis)

Work proceeds one phase task at a time. Full methodology:
[github.com/Mussiefekadu/praxis](https://github.com/Mussiefekadu/praxis).

```
  📝 /praxis:plan TXX
        ↓
  🛠️  /praxis:execute TXX
        ↓
  ✅ /praxis:complete TXX → push (default) + Manual test → next <stub-stem> branch
```

See [`planning/phases/INDEX.md`](planning/phases/INDEX.md).

## 🔒 Security / invariants

- {{INVARIANT_1}}
- {{INVARIANT_2}}

## 📜 License / attribution

{{LICENSE_NOTE}}
