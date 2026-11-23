---
title: feature.panel-order-summary
type: feature-component
status: ready
last-updated: 2026-10-02
figma-nodes:
  file: "https://www.figma.com/design/EXAMPLEKEY/Stockroom"
  nodes: ["88:2051"]
figma-version: "2214508810"
---

**Role:** Shows the items in the employee's current request, the total cost and whether the request needs manager approval, so they can review it before submitting.

**Builds on:** `ui.panel.bundle`. Adds the item list, totals, the approval notice and the submit action.

---

## Content and copy

| Element | Copy | Notes |
|---|---|---|
| Heading | "Your request" | |
| Item count | "{count} item" / "{count} items" | Pluralized |
| Total label | "Estimated total" | |
| Approval notice | "Requests over {threshold} need manager approval." | Shown only when the total exceeds the threshold |
| Empty message | "Add items to start a request." | |
| Submit action | "Submit request" | `ui.button`, `intent=primary` |
| Error message | "We couldn't load your request. Try again." | With a "Try again" `ui.button` |

---

## Data contract

| Field | Type | Required | Source | Notes |
|---|---|---|---|---|
| `items[]` | list | yes | Request API | Empty list is valid |
| `items[].name` | string | yes | Catalog | Maximum 60 characters; longer names truncate with an ellipsis and show in full on hover and focus |
| `items[].quantity` | integer ≥ 1 | yes | User input | |
| `items[].unitCost` | money | yes | Catalog | |
| `total` | money | yes | Derived | Sum of quantity × unit cost |
| `approvalThreshold` | money | yes | Policy API | |
| `currency` | ISO 4217 code | yes | User profile | |

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | Request has items | List, total and submit action shown | — |
| empty | `items` is empty | Empty message shown; submit action disabled | Default: In-place content change |
| loading | Request data requested | Placeholder rows; submit action disabled | Default: Loading |
| error | Request data fails to load | Error message and "Try again"; submit action hidden | Default: In-place content change |
| needs-approval | `total` > `approvalThreshold` | Approval notice shown above the submit action | Default: Disclosure — expand; exit: Disclosure — collapse |
| submitting | Submit action pressed | Submit action follows the `ui.button` loading state; list is read-only | None — `ui.button` owns the loading motion |

---

## Behavior

- Items are listed in the order they were added, newest last. Order is not configurable.
- The total MUST update immediately when an item's quantity changes elsewhere on the screen.
- Money is formatted in the user's currency and locale.
- Submitting emits `submit-request` with the request ID; navigation after submitting belongs to `flow.submit-request`.

---

## Analytics events

| Event | Fires when | Properties |
|---|---|---|
| `order_summary_submit_clicked` | Submit action pressed | `itemCount`, `total`, `needsApproval` |

---

## Error handling

| Error | Cause | User sees | Recovery |
|---|---|---|---|
| Load failed | Request API error or timeout | "We couldn't load your request. Try again." | "Try again" reloads the request data |

Submission errors are handled by the screen (see `screen.request`).

---

## Content and localization

- Allow 40% text expansion for headings and labels.
- Currency, numbers and plurals use the user's locale.

---

## Generative use

- **Available as:** `standalone`.
- **When to use:** the person asks what's in a request, or what it costs.
- **Data contract:** the fields in Data contract above, for one request. Every item, quantity, unit cost and the total MUST come from the request's data, never invented.
- **Variants it may pick:** none; it has no variants.
- **When activated:** none of its actions run from generated content; submitting happens on `screen.request`.
- **Never:** show a request that isn't the person's own, or change quantities from generated content.
- **Worked examples:** a request with "Laptop stand" × 1 at $45 and "USB-C dock" × 2 at $120, threshold $500 → the panel lists both items, "2 items" and an estimated total of $285, with no approval notice.

---

## Accessibility

- The panel is a landmark region labeled "Your request".
- Changes to the total and the appearance of the approval notice are announced politely to assistive technology.
- Truncated item names are available in full to assistive technology.

---

## Acceptance criteria

The component in isolation, given its inputs.

- **AC-1:** **Given** a request with three items, **when** the panel renders, **then** it MUST list the three items in the order added and show "3 items" and the correct estimated total.
- **AC-2:** **Given** an empty request, **when** the panel renders, **then** it MUST show "Add items to start a request." and the submit action MUST be disabled.
- **AC-3:** **Given** a total above the approval threshold, **when** the panel renders, **then** the approval notice MUST be shown with the threshold in the user's currency.
- **AC-4:** **Given** the request data fails to load, **when** the panel renders, **then** it MUST show the error message and "Try again", and MUST NOT show the submit action.
- **AC-5:** **Given** an item's quantity changes, **when** the change is applied, **then** the total MUST update and the change MUST be announced.
