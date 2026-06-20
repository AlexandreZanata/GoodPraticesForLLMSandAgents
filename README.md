# Good Practices for LLMs and Agents

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Harness CI](https://github.com/AlexandreZanata/GoodPraticesForLLMSandAgents/actions/workflows/harness.yml/badge.svg)](https://github.com/AlexandreZanata/GoodPraticesForLLMSandAgents/actions/workflows/harness.yml)

Open-source **Agent Harness** — enterprise-grade, stack-agnostic rules for coding agents and LLM-assisted projects.

**For coding agents:** start with **[AGENTS.md](AGENTS.md)** — universal entry point (Cursor, Claude Code, Codex, etc.).

**Language policy:** 100% English — code, docs, comments, commits, and agent output.

## What this is

A modular rule library + tooling that gives AI coding agents:

- Layered domain-driven architecture constraints
- OWASP Top 10:2025-aligned security rules (web/API, current through 2026)
- OWASP Agentic Top 10:2026 (ASI01–ASI10) for tool-using AI agents
- TDD pyramid and coverage gates
- Token-efficient conditional loading (load 2–6 rules, not 61)

Use as a **git submodule**, **copy**, or **fork** in any project.

## Repository layout

```
rules/                  # Modular rule files + manifest (open source)
AGENTS.md               # Universal entry point for all coding agents
├── manifest.yaml       # Trigger index for conditional loading
├── AGENT-CORE-PRINCIPLES.md
└── 00-core/ … 11-documentation-and-glossary/

harness/                # Install + resolve tooling
├── requirements.txt    # Python deps (PyYAML)
├── install.sh          # Install into any project
├── resolve-rules.sh    # Keywords → rule files
└── inject-frontmatter.py

.cursor/rules/          # Cursor alwaysApply entry points
```

## Quick start

```bash
# Python deps for harness scripts (resolve-rules, inject-frontmatter)
pip install -r harness/requirements.txt

# Resolve rules for an API + auth task
./harness/resolve-rules.sh api endpoint auth

# Install into another project
./harness/install.sh /path/to/your-project
```

Open in **Cursor** — `.cursor/rules/` applies automatically.

Full harness docs: [harness/README.md](harness/README.md)

## Token economy (built in)

| Technique | Why |
|-----------|-----|
| Modular files + `triggers` frontmatter | Load only relevant rules per task |
| Plain technical English | BPE-efficient for LLMs |
| Short imperatives | `Never hardcode secrets` > paragraph |
| Bullets/tables | Dense, parseable |
| Reference, don't repeat | Glossary defines terms once |

See `rules/09-ai-agent-specific/token-economy.md`.

## Core principles (summary)

- **Layers:** Interfaces → Application → Domain ← Infrastructure
- **Domain:** business rules, state machines, immutable events
- **Security:** OWASP Top 10:2025-aligned rules + OWASP Agentic 2026 (ASI01–ASI10)
- **Testing:** 75% unit / 20% integration / 5% E2E; Domain ≥ 90% coverage
- **AI agents:** Application Layer only; read-only; human confirm to persist

Full contract: [rules/AGENT-CORE-PRINCIPLES.md](rules/AGENT-CORE-PRINCIPLES.md)

## Install in your project

```bash
# New AI-assisted project (docs templates + harness)
./harness/bootstrap-project.sh /path/to/new-project

# Existing project (harness only)
git submodule add https://github.com/AlexandreZanata/GoodPraticesForLLMSandAgents.git .agent-harness
./.agent-harness/harness/install.sh . --symlink
```

When in doubt between existing code and these rules, **the rules prevail** — unless explicitly overridden for a task.

## Optional local overrides

Add project-specific rules in `.local/` (gitignored) — layers on top without forking.

## License

MIT — see [LICENSE](LICENSE).
