# Cursor rules (Agent Harness)

This folder is merged into target projects by `harness/install.sh`.

## Always applied

| File | Purpose |
|------|---------|
| `english-only.mdc` | English in all artifacts |
| `agent-core-principles.mdc` | Architecture contract summary |
| `size-and-complexity-limits.mdc` | **80 lines/function, 200 lines/file, cyclomatic ≤10** |
| `test-contract-policy.mdc` | **Read before ANY test** — contract-first, never mirror code |
| `context-discipline.mdc` | Load only what the task needs |
| `token-economy.mdc` | Token-efficient agent behavior |
| `ponytail.mdc` | YAGNI / minimal implementation ([Ponytail](https://github.com/DietrichGebert/ponytail), MIT) |

## Task-scoped (generated)

`generate-task-rules.sh` writes `_task-active.mdc` — delete when the task is done.

## Ponytail (static rules)

Always-on via `ponytail.mdc`. Task-scoped detail:

```bash
./harness/resolve-rules.sh yagni minimal ponytail
```

Attribution: [THIRD_PARTY_NOTICES.md](../THIRD_PARTY_NOTICES.md)
