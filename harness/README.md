# Agent Harness

Open-source harness for AI-assisted software projects. Drop into any repo to give coding agents enterprise-grade, token-efficient rules.

## Quick start (this repo)

Open in Cursor — `.cursor/rules/` loads automatically.

Resolve rules for a task:

```bash
./harness/resolve-rules.sh api endpoint auth
```

## Install in another project

### Option A — Copy (standalone)

```bash
git clone https://github.com/<you>/GoodPracticesForLLMsAndAgents.git
./GoodPracticesForLLMsAndAgents/harness/install.sh /path/to/your-project
```

Installs:
- `your-project/agent-rules/` — full rule library
- `your-project/agent-harness/` — resolve + utilities
- `your-project/.cursor/rules/` — Cursor entry points (merged)

### Option B — Submodule (recommended)

```bash
cd /path/to/your-project
git submodule add https://github.com/<you>/GoodPracticesForLLMsAndAgents.git .agent-harness
./.agent-harness/harness/install.sh . --symlink
```

Symlink keeps rules in sync with submodule updates.

### Option C — Cursor only

```bash
./harness/install.sh /path/to/your-project --cursor-only
```

Copies only `.cursor/rules/` — use when rules live elsewhere.

## Conditional loading (token economy)

Each rule file has YAML frontmatter with `triggers`:

```yaml
---
id: sec.authz
triggers:
  - authorization
  - authz
  - bola
alwaysApply: false
---
```

**Agent protocol:** match task keywords to triggers. Load 2–6 files, not all 61.

| Technique | Effect |
|-----------|--------|
| Modular files | Auth task → load `authorization.md` only |
| Plain English imperatives | Fewer tokens, clearer execution |
| Bullets/tables | Dense without prose overhead |
| Reference over repeat | Glossary defines term once |
| One-line limits | `Max function length: 30 lines` |

Base rules always loaded (see `rules/manifest.yaml` → `always_apply`):

- `AGENT-CORE-PRINCIPLES.md`
- `token-economy.md`
- `anti-hallucination.md`

## Maintenance

After editing `rules/manifest.yaml`:

```bash
python3 harness/inject-frontmatter.py
```

Requires PyYAML: `pip install pyyaml`

## Compatible agents

| Agent | Integration |
|-------|-------------|
| **Cursor** | `.cursor/rules/*.mdc` (alwaysApply) + load `rules/` on demand |
| **Claude Code / CLI** | Point to resolved rule files from `resolve-rules.sh` |
| **Custom pipeline** | Parse `manifest.yaml` triggers in your orchestrator |

## Optional local overrides

Project-specific rules not in harness: add to `.local/` (gitignored) — they layer on top without forking the harness.
