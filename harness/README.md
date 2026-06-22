# Agent Harness

Open-source harness for AI-assisted software projects. Drop into any repo to give coding agents enterprise-grade, token-efficient rules.

## Quick start (this repo)

Install Python dependencies (required for `resolve-rules.sh` and `inject-frontmatter.py`):

```bash
pip install -r harness/requirements.txt
```

Open in Cursor — `.cursor/rules/` loads automatically.

Resolve rules for a task:

```bash
./harness/rules-path.sh
./harness/resolve-rules.sh api endpoint auth
```

Paths are configured in `harness/harness.config.yaml` (`rules/` here; `agent-rules/` after install into another project).

### Cursor task rule (optional)

```bash
./harness/generate-task-rules.sh api endpoint auth
./harness/generate-task-rules.sh --clean   # when task done
```

Writes `.cursor/rules/_task-active.mdc` from manifest triggers.

## Install in another project

### Option A — Copy (standalone)

```bash
git clone https://github.com/AlexandreZanata/GoodPraticesForLLMSandAgents.git
./GoodPraticesForLLMSandAgents/harness/install.sh /path/to/your-project
```

Installs:
- `your-project/agent-rules/` — full rule library
- `your-project/agent-harness/` — resolve + utilities
- `your-project/agent-integrations/headroom/` — Headroom setup (Apache 2.0, optional `pip install`)
- `your-project/.cursor/rules/` — Cursor entry points (merged, includes `headroom-integration.mdc`)
- `your-project/THIRD_PARTY_NOTICES.md` — Apache 2.0 attribution for Headroom

Skip Headroom integration: `./harness/install.sh /path/to/project --skip-headroom`

### Option B — Submodule (recommended)

```bash
cd /path/to/your-project
git submodule add https://github.com/AlexandreZanata/GoodPraticesForLLMSandAgents.git .agent-harness
./.agent-harness/harness/install.sh . --symlink
```

Symlink keeps rules in sync with submodule updates.

### Option C — Cursor only

```bash
./harness/install.sh /path/to/your-project --cursor-only
```

Copies only `.cursor/rules/` — use when rules live elsewhere.

### Option D — New project bootstrap (templates + harness)

```bash
./harness/bootstrap-project.sh /path/to/new-project
# Or symlink mode: ./harness/bootstrap-project.sh /path/to/new-project --symlink
```

Creates `docs/` templates (glossary, API contract, use case, checklist) and installs the harness + Headroom integration docs.

## Headroom (context compression)

Optional third-party layer — [Headroom](https://github.com/headroomlabs-ai/headroom) (Apache 2.0).

| Location (harness repo) | Location (installed project) |
|-------------------------|------------------------------|
| `integrations/headroom/` | `agent-integrations/headroom/` |

```bash
# One-time per machine (Python 3.10+)
./integrations/headroom/setup.sh
./integrations/headroom/setup.sh --wrap    # prints Cursor config to paste

# Installed project
./agent-integrations/headroom/setup.sh --wrap
```

**Modes:**
- `headroom wrap cursor` — paste config once in Cursor settings
- `headroom proxy --port 8787` — drop-in proxy, any OpenAI-compatible client

Attribution: [integrations/headroom/ATTRIBUTION.md](../integrations/headroom/ATTRIBUTION.md) · [THIRD_PARTY_NOTICES.md](../THIRD_PARTY_NOTICES.md)

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
pip install -r harness/requirements.txt
python3 harness/inject-frontmatter.py
./harness/tests/smoke.sh
```

Run **`./harness/tests/smoke.sh` before every release** (or after changing manifest, resolve-rules, install, or bootstrap scripts).

CI runs the same checks on every push/PR to `main` (see `.github/workflows/harness.yml`).

## Compatible agents

| Agent | Integration |
|-------|-------------|
| **Cursor** | `.cursor/rules/*.mdc` (alwaysApply) + load `rules/` on demand + optional Headroom wrap |
| **Claude Code / CLI** | Point to resolved rule files from `resolve-rules.sh` |
| **Custom pipeline** | Parse `manifest.yaml` triggers in your orchestrator |

## Optional local overrides

Project-specific rules not in harness: add to `.local/` (gitignored) — they layer on top without forking the harness.
