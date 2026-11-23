---
title: flow.submit-request
type: flow
status: ready
last-updated: 2026-10-02
---

- **Purpose:** An employee requests equipment or supplies and gets it into the approval or fulfilment queue.
- **Actor:** Employee
- **Trigger:** The employee selects "New request" in the side menu.
- **Preconditions:** The employee is signed in and has no request currently being submitted.

---

## Steps

| # | Actor | Screen | Action | System response | Next |
|---|---|---|---|---|---|
| 1 | Employee | `screen.request` | Opens a new request | Creates a draft and shows the catalog and an empty order summary | 2 |
| 2 | Employee | `screen.request` | Adds one or more items and sets quantities | Updates the order summary and total | 3 |
| 3 | Employee | `screen.request` | Selects "Submit request" | Validates the draft and submits it | 4 |
| 4 | System | | Routes the request | Total at or below the approval threshold: sends to fulfilment | 5 |
| 5 | Employee | `screen.request-confirmation` | Views the confirmation | Shows the request number and next steps | end |

---

## Branches

| Branch | From step | Condition | Outcome | Next |
|---|---|---|---|---|
| 2a | 2 | Employee leaves the screen with items in the draft and chooses "Keep draft" in the discard confirmation | Stays on the screen with the draft unchanged | 2 |
| 2b | 2 | Employee leaves the screen with items in the draft and chooses "Discard" in the discard confirmation | Deletes the draft | end |
| 3a | 3 | An item became unavailable since it was added | The unavailable item is flagged in the order summary; submission stops | 2 |
| 3b | 3 | Submission fails (network or server error) | Submission stops; `screen.request` handles the error | 3 |
| 4a | 4 | Total above the approval threshold | Sends to the employee's manager for approval | 5 |

---

## Postconditions

- **Success (step 5):** a submitted request exists with status "In fulfilment" (or "Awaiting approval" via branch 4a), and the draft no longer exists.
- **Discarded (branch 2b):** no draft and no submitted request exist.

---

## Acceptance criteria

The journey only. Criteria for `screen.request`, `screen.request-confirmation` and their components apply and are not restated.

- **AC-1 (steps 1–5):** **Given** a signed-in employee, **when** they add items totalling at or below the threshold and submit, **then** they MUST reach `screen.request-confirmation` and the request MUST be "In fulfilment".
- **AC-2 (branch 4a):** **Given** a total above the threshold, **when** the employee submits, **then** they MUST reach the confirmation and the request MUST be "Awaiting approval".
- **AC-3 (branch 3a):** **Given** an item became unavailable, **when** the employee submits, **then** submission MUST stop, and the employee MUST be returned to step 2 with the item flagged.
- **AC-4 (branch 3b):** **Given** submission fails, **when** the error returns, **then** the employee MUST remain at step 3 on `screen.request`.
- **AC-5 (state carried):** **Given** items added at step 2, **when** the employee reaches step 5, **then** the confirmation MUST list the same items and quantities.
- **AC-6 (branch 2b):** **Given** a draft with items, **when** the employee chooses "Discard", **then** the draft MUST be deleted and a request MUST NOT be created.
