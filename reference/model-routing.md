# Model routing

**In short:** pick the cheapest model and effort that can do the job well. Name both when you launch a worker. If the result is poor, go up one step and retry. A model the human names wins.

## Two dials
- **Model** sets the tier: how capable the worker is.
- **Effort** sets how hard it thinks inside that tier (in Claude Code: `low`, `medium`, `high`, `xhigh`, `max`; some models step down unsupported levels, and the smallest tier may ignore effort).
A mechanical rename at maximum effort is waste. An architecture call at low effort is a wrong answer, cheaply.

## Job type to model and effort
| Job type | Examples | Model | Effort |
|---|---|---|---|
| Mechanical | Rename, reformat, move files, apply a written diff, count, extract fields | Haiku (smallest tier) | none or low |
| Lookup and search | Find where X is defined, list callers, summarize a file | Haiku or Sonnet | low |
| Standard build | Feature from a clear spec, tests, docs, fixtures, small bug with known cause | Sonnet (mid tier) | low to medium |
| Judgment | Plan, architecture, debugging an unknown cause, code review, security review | Opus (strong tier) | low to medium |
| Hardest | Bug that survived two attempts, risky migration, design with many tradeoffs | The top tier your plan offers (for example Fable), or Opus | high |
| Main orchestrator | Planning, checking, merging, talking to the human | Strong tier | the human's choice |

Adjust the table to the models your team has. Keep one row per job type.

## Escalation ladder
1. Poor or wrong result: first ask whether the brief was the problem (missing context, unclear done). Fix the brief.
2. Same job, same brief, still poor: raise effort one step.
3. Still poor: raise the model one tier, effort back to low or medium.
4. Still poor at the top tier: stop. The problem is probably an unknown in the brief or the human's goal. Ask, or run a gaps check (`research-first.md`).
Never run the same model, effort and brief a third time.

## Rules
- State model and effort in the launch line: `Launching worker (sonnet, effort low): add CSV export`.
- Set them explicitly on every launch. In Claude Code, a subagent file can set a default in its frontmatter (`model:` and `effort:`); a per-launch value overrides it.
- Do not ask an agent which model it runs on. It reads its instructions and repeats them. Check the session log or the tool's usage view instead.
- A model the human names in the prompt wins for that task.
- A missing or wrong model or effort is reported, never silently swapped.
- Do not switch the main session's model mid-session; it throws away the cache. Delegate instead.

## Cost notes
- Bigger models and higher effort cost more per token and take longer. Check your provider's current price page; do not hard-code prices in rules.
- A fork copies the whole conversation, so even a one-line job on a long session is expensive. Use a fresh subagent with a short brief when context is not needed, or a cheap model for the fork.
- Each worker's report lands in the main context. Ask for short final messages and full detail in files.
- Many parallel workers spend tokens in a burst. Launch them when the work is truly independent, not to fill slots.
