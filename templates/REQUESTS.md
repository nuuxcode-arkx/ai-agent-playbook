# REQUESTS

Project: `<PROJECT_NAME>`
Method: `reference/request-tracking.md`. One row per request or sub-request. A row is `done` only when Proof is filled.

## Now
| Open | Running | Blocked | Done | Not done |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |

## Tracker
| ID | Request | Definition of done | How checked | Owner | Status | Proof | Notes |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## Approvals log
| Date | Action and target | Human's exact words |
|---|---|---|
| | | |

---

## Example (delete this section once you start)

Prompt P1, from the human: "The signup form breaks on phones and the welcome email has a typo. Also can you add a CSV export to the admin users page?"

Goal: signup works on phones, the welcome email is correct, admins can export users as CSV.

| ID | Request | Definition of done | How checked | Owner | Status | Proof | Notes |
|---|---|---|---|---|---|---|---|
| P1.1 | Signup form breaks on phones | Form fits a 390 px screen and submits | Playwright at 390, 820, 1440; submit with a test account | worker-a (sonnet, low) | done | `ui-check` exit 0; `shots/signup-390.png` | |
| P1.1a | Find the cause | Cause named with file and line | Read CSS and reproduce at 390 px | worker-a | done | `src/signup.css:42` fixed width 600px | |
| P1.1b | Fix and test | No horizontal scroll, submit returns 201 | Same as P1.1 | worker-a | done | see P1.1 | |
| P1.2 | Typo in welcome email | Subject and body read correctly | Render template in test; read output | main (quick edit) | done | `npm test -- email` exit 0 | |
| P1.3 | CSV export on admin users page | Admin clicks Export, gets a CSV of all users with the listed columns | API test plus a browser click-through as a test admin | worker-b (sonnet, medium) | running | | |
| P1.3a | Pick columns | Columns agreed | Human answer | main | blocked | | Waiting: asked the human whether to include phone numbers |
| P1.3b | Endpoint and button | Endpoint returns CSV, button downloads it | `curl` returns `text/csv`; Playwright click | worker-b | todo | | Depends on P1.3a |
