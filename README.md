# ai-agent-playbook

**A drop-in rule set for teams that use AI coding agents (Claude Code, Codex and tools that read `AGENTS.md`).**

It gives the agent a working method: track every request, keep the main session free by handing jobs to workers, pick the right model for each job, research before building, test screens in a real browser, and prove work before calling it done.

## Who it is for
Teams new to `CLAUDE.md` or `AGENTS.md` who want a sensible starting point, and teams whose agents keep dropping asks, claiming "done" too early, or doing everything one step at a time.

## The six ideas
1. **Track every request.** The agent splits each prompt into requests and sub-requests, writes them in a tracker with a definition of done, and checks each one against proof before it stops. Nothing drops silently.
2. **The main session orchestrates.** If the main session does the work itself, it is blocked, and every new prompt becomes an interruption. Instead, it hands each job to a subagent, fork or background worker, runs independent jobs in parallel, and stays free to take new prompts. It reads and checks results, then reports.
3. **Right model, right effort.** Small models for mechanical work, mid models for standard work, strong models for judgment, the top tier for the hardest problems. Poor result: go up a step and retry.
4. **Research first.** Before building something big from scratch, search the codebase, GitHub and the web for existing work. Reuse within the licence.
5. **UI that a non-technical person understands.** A concrete checklist for colour, type, spacing and layout, and a real-browser test at phone, tablet and desktop widths with every screenshot looked at.
6. **Habits that keep agents honest.** Precise status words, root-cause thinking, confirm-before-delete/push/send/spend, secrets in environment variables, read back every change, short plain reports, and turning each correction into a rule.

## File map
| File | What it is |
|---|---|
| `CLAUDE.md` | The core rules for Claude Code (about 100 lines). Always loaded. |
| `AGENTS.md` | The same rules for Codex and other tools that read `AGENTS.md`. |
| `reference/request-tracking.md` | Tracker columns, the method, and the done-check before ending a turn. |
| `reference/orchestrator-and-workers.md` | Why main stays free, queue building, fork vs subagent vs background worker, parallel safety, checking results. |
| `reference/model-routing.md` | Job type to model and effort, the escalation ladder, cost notes. |
| `reference/research-first.md` | Where to search, what to record, licences, handling unknowns. |
| `reference/ui-ux-standard.md` | UI checklist and the browser test procedure, with a small Playwright script. |
| `reference/verification.md` | Status words, proof habits, the gate, the repo standard. |
| `reference/safety.md` | What needs a human yes, secrets, customer data, outbound sends, untrusted input. |
| `reference/communication.md` | Status shape, plain language, decisions, drafts. |
| `templates/REQUESTS.md` | Empty tracker with one filled example. |
| `templates/worker-brief.md` | The brief every worker gets. |
| `templates/project-CLAUDE.md` | Short per-repo file: stack, commands, conventions, gate. |

## Install for Claude Code
Claude Code reads `~/.claude/CLAUDE.md` in every project (your personal rules) and `./CLAUDE.md` or `./.claude/CLAUDE.md` in a repo (the team's rules for that repo). Both load together.

**Option A: for you, in every project**
```bash
git clone https://github.com/nuuxcode-arkx/ai-agent-playbook ~/ai-agent-playbook
cp ~/ai-agent-playbook/CLAUDE.md ~/.claude/CLAUDE.md          # back up an existing one first
cp -r ~/ai-agent-playbook/reference ~/ai-agent-playbook/templates ~/.claude/
```
Then edit `~/.claude/CLAUDE.md` and change each `reference/...` and `templates/...` path to `~/.claude/reference/...` and `~/.claude/templates/...`, so the agent finds them from any project.

**Option B: for a team, in one repo**
```bash
cp -r CLAUDE.md reference templates <your-repo>/           # or put them under <your-repo>/.claude/
cp templates/project-CLAUDE.md <your-repo>/.claude/CLAUDE.md  # fill in the repo specifics
```
Commit them so the whole team gets the same rules.

Notes:
- `reference/` files are read on demand, so they cost no context until needed. To load one in every session, add an import line such as `@reference/safety.md` to `CLAUDE.md`. Imports load in full, so use them sparingly. Keep each `CLAUDE.md` under about 200 lines.
- Put personal, uncommitted tweaks for a repo in `CLAUDE.local.md` and add it to `.gitignore`.
- Subagents live in `.claude/agents/` (project) or `~/.claude/agents/` (user). Set `model:` and `effort:` in their frontmatter to give each a default; see `reference/model-routing.md`.
- Rules in `CLAUDE.md` are guidance, not enforcement. For the rules that must never break (force push, broad deletes, secrets in files), add hooks.

## Install for Codex and other AGENTS.md tools
- Global: copy `AGENTS.md` to `~/.codex/AGENTS.md` and `reference/` and `templates/` next to it, then adjust the paths.
- Per repo: copy `AGENTS.md` to the repo root, with `reference/` and `templates/` beside it.
- If you want one file for both tools, keep `AGENTS.md` and make `CLAUDE.md` a single line: `@AGENTS.md`. Claude Code then loads it through the import.

## How to adapt it
Search for `<` followed by a capital letter to find every placeholder:
| Placeholder | Fill with |
|---|---|
| `<YOUR_NAME>`, `<YOUR_EMAIL>` | The git identity the team commits with |
| `<PROJECT_NAME>` | The repo or product name |
| `<INSTALL_COMMAND>`, `<DEV_COMMAND>`, `<BUILD_COMMAND>`, `<TEST_COMMAND>`, `<LINT_COMMAND>` | Your real commands |
| `<GATE_COMMAND>` | The one command that must pass before "done" (often lint + typecheck + tests) |
| `<SEED_FILE>`, `<TEST_INBOX>`, `<YOUR_DOMAIN>` | Where test accounts come from; an inbox you control for test emails |
| `<PORT>`, `<VAR_NAME>`, `<MECHANISM>`, `<SHORT_JOB_NAME>` | Short notes inside `templates/`; each one says what to write |

Then:
- Edit the model table in `reference/model-routing.md` to the models your team has.
- Delete rules that do not fit. Shorter rule files are followed better.
- Add your own rules the same way: imperative, checkable, generic.

## Start small
You do not need all of it on day one.
1. **Week 1:** adopt request tracking (rule 1). Copy `templates/REQUESTS.md` into one repo and keep only the "Requests" section of `CLAUDE.md`.
2. **Week 2:** add the orchestrator rule (rule 2) and `templates/worker-brief.md`. Watch how much faster mixed prompts move.
3. **Then:** add verification and safety, then the rest, one section at a time.

## Licence
MIT. See `LICENSE`.
