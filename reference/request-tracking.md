# Request tracking

**In short:** every prompt is split into requests and sub-requests. Each one gets a definition of done and a way to check it. Before the agent ends a turn, it checks every row against proof. The tracker shows the state of the work in one glance.

## Why
A prompt often holds three or four asks. Agents answer the first or the loudest and drop the rest. A written list with a done-check for each item makes a dropped ask visible.

## When
- Every prompt that asks for work. A plain question needs no row.
- A prompt that arrives in the middle of a task: add its rows first, then resume.
- After compaction or a resume: read the tracker before anything else.

## Where
- Default: `REQUESTS.md` at the project root (copy `templates/REQUESTS.md`).
- Teams that live in spreadsheets can use one sheet with the same columns.
- One tracker per project. Do not keep a second copy in chat or in another file; other views (status reports) are built from it.
- If your tool has a built-in task list, use it for the live view, and keep the tracker file as the record.

## Columns
| Column | Meaning |
|---|---|
| ID | `P3` for the third prompt, `P3.2` for its second request, `P3.2a` for a sub-request. Never reuse an ID. |
| Request | The ask in the human's words, shortened. Keep their key words. |
| Definition of done | What must be true when it is finished. One or two checkable sentences. |
| How checked | The command, test, screenshot or URL that proves it. Written before the work starts. |
| Owner | `main`, or the worker name, plus model and effort. |
| Status | `todo`, `running`, `done`, `not done`, `blocked`, `cancelled`, `deferred`. |
| Proof | The evidence: command and exit code, file path, screenshot path, link. Empty proof means not done. |
| Notes | Blocker reason, the human's words for a cancel, follow-ups. |

## The method
1. **Read the whole prompt.** Underline every verb that asks for something: fix, add, check, explain, send.
2. **Write the goal** in one line: what the human will see when everything is finished.
3. **Split** into requests (each a separate outcome) and sub-requests (steps or parts of one outcome). Keep implicit asks too: "and make sure it works on mobile" is its own row.
4. **Write the definition of done and how checked** for each row before starting. If you cannot say how you will check it, the ask is unclear: resolve it with the most likely reading and note the assumption, or ask if the fork is hard to undo.
5. **Queue.** Mark which rows can run in parallel. See `orchestrator-and-workers.md`.
6. **Update as you go.** A row moves to `done` only when its Proof column is filled.
7. **Done-check before ending the turn** (below).

## Done-check procedure
Run this before the last message of every turn that did work.
1. Open the tracker. Count rows by status.
2. For each row marked `done`: re-open the proof. Does it still match the current state? (A later change can break an earlier fix.) If not, move it back.
3. For each row not `done`: it must have a status and a reason (`blocked: waiting for API key`, `deferred: human said later`, `cancelled: human said "drop it"`).
4. Compare the rows with the original prompt one more time. Any ask with no row is a dropped ask: add it now.
5. In the final message, lead with what is not done, then what is done, each with its ID.

## Rules
- Status words follow `verification.md`. "Done" needs proof that a real check ran.
- Never mark a row done from memory of the code.
- A cancel needs the human's words in Notes. The agent never cancels a row on its own.
- When an AI tool rewrites the human's prompt, diff the rewrite against the original and keep every item.
- If it is unclear whether a message is one request or several, ask once.
- Enforce with a hook if you can: a stop-time check that fails when any row is `running` or `todo` without a note.
