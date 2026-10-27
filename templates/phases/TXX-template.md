# TXX — {{Short Title}}

**Status**: Pending  
**Parent INDEX**: [INDEX.md](./INDEX.md)  
**Depends-on**: {{T0prev or —}}  
**Next**: {{T0next or —}}  
**Layer**: L?

## Description

{{One paragraph: what this task delivers and why it exists in the layer order.}}

## Status History

| Timestamp | Event | From | To | Details | User |
| --------- | ----- | ---- | -- | ------- | ---- |
| | created | — | Pending | stub seeded by bootstrap | |

## Requirements

- [ ] {{REQ_1}}
- [ ] {{REQ_2}}

## Implementation Plan

*(Filled by `/praxis:plan` — do not invent during bootstrap beyond high-level notes.)*

### High-level notes (bootstrap)

- {{NOTE_1}}
- Reference: {{PATH_OR_URL}}

## Execution plan (filled by /praxis:plan)

**Date:**  
**Codebase snapshot:**  
**Execute model:** small/default | large (only if justified)

### Context for executor
- …

### Steps
1. … → verify: …

### Tests to add
- …

### Verify commands
- …

### Risks / pitfalls
- …

### Out of scope
- …

### Execute model recommendation
- default (small/cheap) | large — rationale: …

## Test Plan

- {{How to prove this task without needing later layers}}
- Commands: `{{VERIFY}}` (must include **lint + tests**)
- New code: tests required (stack's idiomatic style)

## Acceptance Criteria

- [ ] {{AC_1}}
- [ ] {{AC_2}}
- [ ] Tests added/updated for new behavior
- [ ] Full lint + test verify suite green (auto-fix applied first)
- [ ] Verification commands recorded and passing
- [ ] No secrets committed

## Verification

*(Filled by `/praxis:execute`; re-confirmed by `/praxis:complete`)*

## Files Modified

*(Filled by `/praxis:execute`)*

## Manual test (for humans)

*(Filled by `/praxis:complete` — runnable commands + what to look for, or
`Nothing to test — <why>`)*

## Learnings

*(Filled by `/praxis:complete` / dialectic — link rule entries)*

## Reality notes

*(Amended by upstream `/praxis:complete` if prior tasks changed assumptions)*
