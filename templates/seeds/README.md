# Praxis seeds

`/praxis:init` copies these into `planning/` on the target project. **`/praxis:bootstrap`**
must adapt them to the **repo root** (seeds under `planning/` alone are not enough).

| Seed folder | Installs as | Bootstrap adapts to |
| ----------- | ----------- | ------------------- |
| [`readme/`](readme/) | `planning/README-SEED.md` | Root `README.md` |
| [`gitignore/`](gitignore/) | `planning/gitignore-SEED` | Root `.gitignore` |
| [`verify/`](verify/) | `planning/verify-SEED/` | Go projects: root `Makefile`, `lefthook.yml`, `.golangci.yml`. Other stacks: delete and use the stack's own tooling |

**Verify contract:** the hub's Verify gate section names one verify command; it must
mean lint + test (+ build when applicable) — package tests alone are not the gate.
Execute/complete hard-abort if it or its required files are missing. Go seed Makefile also ships `build-all` (multi-platform) — not
part of the default verify gate.
