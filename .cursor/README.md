# Cursor rules (Agent Harness)

This folder is merged into target projects by `harness/install.sh`.

## Always applied

| File | Purpose |
|------|---------|
| `english-only.mdc` | English in all artifacts |
| `agent-core-principles.mdc` | Architecture contract summary |
| `context-discipline.mdc` | Load only what the task needs |
| `token-economy.mdc` | Token-efficient agent behavior |
| `headroom-integration.mdc` | Optional Headroom compression (Apache 2.0) |

## Task-scoped (generated)

`generate-task-rules.sh` writes `_task-active.mdc` — delete when the task is done.

## Headroom setup (human, once per machine)

```bash
./integrations/headroom/setup.sh --wrap
```

Source: https://github.com/headroomlabs-ai/headroom (Apache 2.0)
