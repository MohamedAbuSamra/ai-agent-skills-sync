# Skill Audit

This file classifies skills by scope and location.

## Folder layout

| Folder | Purpose |
|---|---|
| `skills/` | Cross-agent engineering standards — used by Cursor, Claude Code, Codex, Copilot |
| `cursor/skills-cursor/` | Cursor-specific workflow skills |
| `claude/skills/` | Claude Code-specific skills |
| `codex/skills/` | Codex/OpenAI skills |

## skills — global baseline (apply by default)

- `master-engineering-standards`
- `always-apply-standards`
- `modern-development-practices`
- `clean-code-principles` ← naming, functions, comments, error handling, testing (Martin/Pressman)
- `product-software-thinking` ← validate before building, user goals drive tech decisions (cross-domain)
- `testing-patterns`
- `security-best-practices`
- `typescript-best-practices`
- `git-workflow`
- `personal-camelcase-preference`

## skills — conditional reusable (apply when domain matches)

- `api-design-restful`
- `algorithms-data-structures` ← complexity, sorting, graphs, DP, binary search (CLRS/Goodrich)
- `async-concurrency`
- `database-data-modeling`
- `documentation-best-practices`
- `domain-driven-design` ← bounded contexts, aggregates, value objects, domain events
- `dry-solid-principles`
- `error-handling-logging`
- `game-design-principles` ← engagement loops, flow state, feedback systems (Schell/Koster/Sylvester)
- `javascript-patterns` ← Good Parts, closures, modules, functional JS (Crockford/Zakas/Elliott)
- `performance-optimization`
- `software-patterns-architecture`
- `ux-product-design` ← affordances, mental models, design sprints, usability (Norman/Maeda/Shneiderman)
- `validation-input-sanitization`
- `vue-frontend-patterns`

## claude/skills — Claude Code specific

- `claude-code-workflow`
- `claude-memory-system`

## Guidance

- New cross-agent skills go in `skills/`.
- Agent-specific behavior goes in that agent's folder.
- Prefer updating an existing shared skill over creating a new one.
- When adding a new agent: create `<agent>/skills/` and wire it in `sync.sh`, `sync.ps1`, `validateSkills.sh`, and `learningUiServer.py`.