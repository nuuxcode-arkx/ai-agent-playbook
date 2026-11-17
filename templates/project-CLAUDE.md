# <PROJECT_NAME>

<One sentence: what this project is and who uses it.>

The team-wide rules live in the user-level `CLAUDE.md` (or `AGENTS.md`). This file adds only what is specific to this repo. Keep it under about 100 lines.

## Stack
- Language and version: <for example Node 22.11, Python 3.12>
- Framework: <name and version>
- Database: <name; local vs staging vs production>
- Hosting: <where it deploys>

## Commands
| Task | Command |
|---|---|
| Install | `<INSTALL_COMMAND>` |
| Dev server | `<DEV_COMMAND>` (port `<PORT>`) |
| Build | `<BUILD_COMMAND>` |
| Test | `<TEST_COMMAND>` |
| Lint and typecheck | `<LINT_COMMAND>` |
| Gate (run before any "done" or commit) | `<GATE_COMMAND>` |

## Layout
| Folder | What lives there |
|---|---|
| `src/` | <...> |
| `tests/` | <...> |
| `docs/` | <product and architecture docs> |

## Conventions
- <naming, file structure, state management, error handling patterns>
- <where new components go; which existing helpers to reuse>
- <test style: where tests live, what a good test looks like>

## Environments and data
- Test accounts: from `<SEED_FILE>`. Never real customer data.
- Outbound sends (email, SMS, webhooks) are disabled in dev by `<MECHANISM>`.
- Environment variable names (never values): `<VAR_NAME>`: <meaning>.

## Standing approvals (optional)
- <for example: "Push to the `staging` branch when the gate is green. Never `main`, never force push.">

## Known traps
- <non-obvious findings that cost time before; add one the moment you find it>
