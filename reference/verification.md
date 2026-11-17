# Verification

**In short:** written, runs, passes and verified mean different things. Every "done" points at a real check against the current state. Assume every result is wrong until ground truth shows it is right.

## Status words
| Word | Means | Example proof |
|---|---|---|
| Written | The code or doc exists. Nothing ran. | File path |
| Runs | It starts without crashing. | Command output |
| Passes | Its tests or gate pass. | Test command plus exit code |
| API-proven | A script or curl call shows the behavior. | Request plus response |
| Verified | Driven like a real user from the entry point (the UI, the CLI, the public endpoint). | Screenshots or recorded steps |
| Deployed, untested | It is live, nobody checked it there. | Deploy record |
| Build-green | The build passed, with no deeper proof. Use it when the team has chosen speed over full checks. | Gate output |

Never use a stronger word than the proof supports. "Done" with no proof in the turn is not done.

## Habits
- Check the result after every action: the log line, the file on disk, the live page, the record in the database. Never report from memory of what the code should do.
- Name the one check that can prove the change wrong, and run it. Typecheck and lint are entry criteria, never exit criteria.
- Test at real scale: real payload sizes, real record counts, the settled state after animations, the new build.
- Prove every guard twice: a case that must be blocked is blocked, and a case that must pass passes.
- An external integration is untested until the real service fired once (in a sandbox or test mode).
- After a deploy, confirm the new build is live (a version marker only the new build has) before you test it.
- Claims of "all" or "complete" need a count against files in the same turn.
- Tag claims in audits and specs: CONFIRMED (file:line or command output), INFERRED (say which probe would prove it), GAP (unknown).
- Read logs and error output before reading source. Compare a broken case with a working one before theorizing.
- A fixed bug gets a test that would have caught it.
- Run a pending check before the next report, even if the human moved on to a new ask.
- If the project has no tests or type checker, say so.

## Failed checks vs permission denials
- A failed test, build or lint is your job: find the cause, fix it, rerun.
- Never weaken an assertion, skip a test or disable a check to make it pass.
- Stop only for a real permission denial. Never route around it through another tool.

## Gate
- Each project names its gate in its own `CLAUDE.md` or `AGENTS.md` (`<GATE_COMMAND>`).
- Run it before any commit or "done". Capture the exit code: `<GATE_COMMAND> > gate.log 2>&1; echo EXIT=$?`. Never pipe a gate through a filter that hides its exit code.
- Before calling a batch of changes finished, get a find-only review from a fresh worker (security, speed, edge cases, UX).

## Repo standard
A repo is not finished until a newcomer can understand it.
- GitHub description: one plain line, what it does and for whom. Homepage link if live. 4 to 8 topics.
- README sections, in order: title and one-sentence summary; status (one honest line); why it exists; what it does (numbered user-level steps); what it will not do; how it works (small diagram, folder table); progress checklist (never tick unproven items); run locally (copy-paste commands, environment variable names and meaning, never values); deploy.
- A non-technical teammate understands the first five sections without help.
- Keep README, product and architecture docs true today; update them in place in the same commit as the change. Git keeps the history.
- No secrets, personal data or internal session notes in the repo.
