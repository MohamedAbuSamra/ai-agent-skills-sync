# ai-agent-skills-sync

Sync AI agent skills (Cursor, Claude Code, Codex, Copilot) across machines with GitHub.

## Contributors

This repo is maintained as a shared multi-agent system. Current contributing agent surfaces:

- `Claude Code` — global rules via `claude/CLAUDE.md` and `claude/skills/`
- `Cursor` — workflow helpers via `cursor/skills-cursor/`
- `Codex` — collaboration workflow via `codex/skills/`
- `Copilot` — bootstrap instructions via `prompts/copilot.md` and stamped repo instructions

## Quick setup (one command)

**macOS / Linux**

```bash
git clone https://github.com/MohamedAbuSamra/ai-agent-skills-sync.git
cd ai-agent-skills-sync
chmod +x ./setup.sh
./setup.sh
```

Then **restart your AI agents** (Cursor, Codex, Claude Code, etc.). Skills from this repo are now active.

**Windows (PowerShell)**

```powershell
git clone https://github.com/MohamedAbuSamra/ai-agent-skills-sync.git
cd ai-agent-skills-sync
.\setup.ps1
```

Then **restart your AI agents** (including Codex if you use it).

---

## Bootstrap any AI agent with one prompt

Every AI agent (Claude, Cursor, Copilot, Codex) can be instantly loaded with your full skills and engineering standards by pasting a bootstrap prompt into the chat.

**Prompts live in `prompts/`:**

| File | Use when |
|---|---|
| [`prompts/universal.md`](prompts/universal.md) | Any agent — paste the prompt block inside it |
| [`prompts/claude.md`](prompts/claude.md) | Claude.ai or a new Claude Code session |
| [`prompts/cursor.md`](prompts/cursor.md) | Cursor AI chat (Cmd+L) |
| [`prompts/codex.md`](prompts/codex.md) | Codex / OpenAI Assistants |
| [`prompts/copilot.md`](prompts/copilot.md) | GitHub Copilot Chat in VS Code |

**How to use:**

1. Open the relevant prompt file
2. Copy the text block between the triple-backtick fences
3. Paste it as your first message in the AI agent chat
4. The agent reads your skills, confirms what it loaded, and is ready to work

Once pasted, the agent will:
- Load `PROJECT-BOOTSTRAP.md`, `SKILL-AUDIT.md`, and the key baseline skills
- Apply `clean-code-principles` and `product-software-thinking` by default
- Know which conditional skills to apply (algorithms, DDD, UX, game design, etc.)
- Know about the learning pipeline and suggest `record-learning.sh` when a pattern emerges

> **Note:** For Claude Code CLI and Cursor, running `./sync.sh to-local` installs skills globally — you won't need the prompt every session. The prompts are most useful for web-based chats or fresh machines.

## What this repo contains

- `skills/` -> cross-agent engineering skills used by all AI agents
- `cursor/skills-cursor/` -> Cursor-specific helper skills
- `codex/skills/` -> Codex skills
- `claude/skills/` -> Claude Code-specific skills
- `claude/CLAUDE.md` -> global Claude Code rules (synced to `~/.claude/CLAUDE.md`)
- `learning/manual/` -> manually captured learnings
- `learning/generated/` -> AI-generated learnings
- `PROJECT-BOOTSTRAP.md` -> short portable start-of-work contract for any repo

## Portable bootstrap

If you want a fast, repo-local file to inspect at the start of work, use `PROJECT-BOOTSTRAP.md`.

It is designed to be:

- short enough to inspect quickly
- portable across repos and chats
- stable enough to act as your default engineering baseline

Use it before falling back to longer prompts or deeper skill files.

## Automated repo bootstrap

If you want new repos to start from your style automatically, stamp local instruction files into them:

```bash
./scripts/install-repo-bootstrap.sh /absolute/path/to/target-repo
```

PowerShell:

```powershell
.\scripts\install-repo-bootstrap.ps1 -TargetDir C:\path\to\target-repo
```

This creates:

- `copilot-instructions.md`
- `AGENTS.md`
- `CLAUDE.md`

That gives each target repo a local instruction surface for all AI agents. For Codex specifically,
`AGENTS.md` is the durable repo-local memory file that should be read before work starts.

## Skill layout

Skills are split by scope:

- **`skills/`** — cross-agent standards. Every AI agent in this repo uses these. New skills that should apply everywhere go here.
- **`cursor/skills-cursor/`** — Cursor-specific workflow skills
- **`claude/skills/`** — Claude Code-specific skills
- **`codex/skills/`** — Codex/OpenAI skills

Current Codex-specific skill:

- `codex-collaboration-workflow` — inspect first, preserve user changes, communicate progress, validate changed paths

Adding a new AI agent: create `<agent>/skills/`, wire it in `sync.sh`, `sync.ps1`, `validateSkills.sh`, and `learningUiServer.py`.

Recommended global baseline (all in `skills/`):

- `master-engineering-standards`
- `always-apply-standards`
- `modern-development-practices`
- `testing-patterns`
- `security-best-practices`
- `typescript-best-practices`

Examples of conditional reusable skills (all in `skills/`):

- `api-design-restful`
- `error-handling-logging`
- `validation-input-sanitization`
- `dry-solid-principles`
- `performance-optimization`
- `database-data-modeling`

See `SKILL-AUDIT.md` for the full classification.

## How it works across projects

- **Skills** are global. After `./setup.sh`, all configured agents (Cursor, Claude Code, Codex) read from their respective skill directories in **every project**. The same skills apply whether you open this repo or another app.
- **Learnings** live only in this repo (`learning/manual/entries.md` and `learning/generated/entries.md`). To add a learning when you're in a **different project**, run the script by path or use an alias:

  ```bash
  # From any folder (replace with your actual path):
  ~/ai-agent-skills-sync/record-learning.sh generated "Title" "What you learned"
  ```

  Optional alias (add to `~/.zshrc` or `~/.bashrc`):

  ```bash
  alias log-learning='~/ai-agent-skills-sync/record-learning.sh'
  # Then from any project: log-learning generated "Title" "Details"
  ```

  Then `git pull` / `git push` from the ai-agent-skills-sync repo to sync learnings across machines.

## Important difference: learning vs skills

- `learning/*` is a draft log (notes and experiments). It does not directly change assistant behavior.
- `skills/*` is active guidance (`SKILL.md`) used by the assistant.
- Use this flow: capture idea in `learning/*` -> validate in real work -> promote to `skills/*`.

Quick rule:

- If the idea is new/unproven, store it in `learning/*`.
- If the idea is stable/reusable, convert it into a skill.

## Restore on another machine

Use the **Quick setup** above (`./setup.sh` or `.\setup.ps1`). Or manually: run `./sync.sh to-local` (or `.\sync.ps1 to-local`) from this repo, then restart your AI agents.

## Update this repo after local changes

Run these from this repo folder:

```bash
./sync.sh to-repo

git add .
git commit -m "chore: sync local skills"
git push
```

For Windows:

```powershell
.\sync.ps1 to-repo
git add .
git commit -m "chore: sync local skills"
git push
```

## Git hooks and CI guard

Install local hooks once per machine:

```bash
chmod +x ./scripts/installHooks.sh
./scripts/installHooks.sh
```

What you get:

- `pre-commit`: runs `scripts/validateSkills.sh`
- `post-commit`: reminds you to push after `SKILL.md` changes
- GitHub Action (`skills-guard`): validates skills on push/PR

## Learning review + promotion commands

Text UI (dashboard):

```bash
./scripts/learning-dashboard.sh 10
```

Local web UI:

```bash
./scripts/run-learning-ui.sh 8765
```

Then open `http://127.0.0.1:8765`.

Review a learning:

```bash
./scripts/review-learning.sh generated <fingerprint> approved "validated in 3 tasks"
```

Promote approved learning into a real skill:

```bash
./scripts/promote-learning.sh generated <fingerprint> my-skill-slug "Short skill description" skills
```

Promote approved learning into an existing skill:

```bash
./scripts/promote-learning-into-existing.sh generated <fingerprint> master-engineering-standards skills
```

In the local web UI, each learning now includes a suggested target skill and a one-click action to promote into that existing skill.

PowerShell equivalents:

```powershell
.\scripts\learning-dashboard.ps1 -Limit 10
.\scripts\run-learning-ui.ps1 -Port 8765
.\scripts\review-learning.ps1 -Source generated -Fingerprint <fingerprint> -Status approved -Reason "validated in 3 tasks"
.\scripts\promote-learning.ps1 -Source generated -Fingerprint <fingerprint> -Slug my-skill-slug -Description "Short skill description" -Target skills
.\scripts\promote-learning-into-existing.ps1 -Source generated -Fingerprint <fingerprint> -SkillSlug master-engineering-standards -Target skills
```

## Learning flow (manual vs generated)

Track daily improvements without mixing the source:

### macOS / Linux

```bash
chmod +x ./record-learning.sh
./record-learning.sh manual "title" "details from your own decision"
./record-learning.sh generated "title" "details from AI suggestion"
```

### Windows (PowerShell)

```powershell
.\record-learning.ps1 manual "title" "details from your own decision"
.\record-learning.ps1 generated "title" "details from AI suggestion"
```

See `LEARNING-FLOW.md` for the full daily/weekly process.

Built-in behavior:

- no duplicate entries (same source+title+details)
- every new entry starts as `status: pending`
- reminder when pending entries reach threshold (default: 5, configurable with `learningReviewThrasholder`)

## Promote a learning into a skill

1. Pick a validated entry from `learning/manual/entries.md` or `learning/generated/entries.md`.
2. Create/update a folder under `skills/` (cross-agent) or the relevant agent folder (`cursor/skills-cursor/`, `claude/skills/`, etc.).
3. Add or update `SKILL.md` with clear instructions and examples.
4. Commit and push.

For cross-agent guidance, prefer extending `skills/master-engineering-standards/` or adding a new skill under `skills/`.

For a quick start in any repo, inspect `PROJECT-BOOTSTRAP.md` first.

## Knowledge sources behind these skills

Skills in this repo are grounded in real-world software and product experience, supplemented by a curated book collection built up over years of engineering and product work.

### Book collection (Google Drive)

The personal book library that informed skill creation across algorithms, design, software craft, game theory, and product thinking:

**[View book collection on Google Drive](https://drive.google.com/drive/folders/17c9DZ8fZLTO_YKArA-GOHn8QXaRYlvCt?usp=sharing)**

Key books by skill domain:

| Skill | Source books |
|---|---|
| `algorithms-data-structures` | *Introduction to Algorithms* (CLRS), *Data Structures & Algorithms in Java* (Goodrich & Tamassia) |
| `clean-code-principles` | *Clean Code* (Robert C. Martin), *Software Engineering: A Practitioner's Approach* (Pressman) |
| `domain-driven-design` | *Domain-Driven Design by Example* (Evans patterns) |
| `game-design-principles` | *Art of Game Design* (Schell), *Theory of Fun* (Koster), *Designing Games* (Sylvester), *Game Design Workshop* (Fullerton) |
| `javascript-patterns` | *JavaScript: The Good Parts* (Crockford), *Professional JS for Web Devs* (Zakas), *Programming JS Applications* (Elliott) |
| `product-software-thinking` | *Design of Everyday Things* (Norman), *Theory of Fun* (Koster), *Design Sprint* (Banfield), *Laws of Simplicity* (Maeda) |
| `ux-product-design` | *Design of Everyday Things* (Norman), *Laws of Simplicity* (Maeda), *Design Sprint* (Banfield), *Designing the User Interface* (Shneiderman) |
| `software-patterns-architecture` | *Domain-Driven Design*, *Design Patterns* (GoF) |

When a new skill is added from this collection, the source book(s) are listed in the skill's `description` frontmatter field.

## Automation limits

This repo can automate review and promotion suggestions inside its own scripts and local UI, but it cannot force every future chat host to auto-run promotion logic on startup.

Practical default:

- global user agent reminds the assistant to suggest promotion opportunities
- local learning UI suggests a target skill automatically
- one-click promotion into an existing skill is available from the UI
