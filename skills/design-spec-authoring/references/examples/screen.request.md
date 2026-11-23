---
title: screen.request
type: screen
status: ready
last-updated: 2026-10-02
figma-nodes:
  file: "https://www.figma.com/design/EXAMPLEKEY/Stockroom"
  nodes: ["150:12", "150:340"]
figma-version: "2214508810"
---

**Purpose:** The employee chooses items for a new request, reviews it and submits it.

**Layout:** `sidebar-content` page shell (`layouts.md`).

---

## Regions and composition

| Region | Components | Notes |
|---|---|---|
| Side menu | `feature.side-menu-request` | "New request" is the current item (`aria-current="page"`) |
| Header | `ui.page-header` | Title "New request" |
| Main | `feature.catalog-picker` | Item search and quantity selection |
| Aside | `feature.panel-order-summary` | Sticky on wide; below the catalog on compact |
| Overlay | `ui.modal.bundle` | Discard confirmation only |
| Toast | `ui.toast` | Submission errors only |

---

## Data contract

| Data | Source | Loaded when | Notes |
|---|---|---|---|
| Current draft request | Request API | On entry | Created if none exists |
| Catalog | Catalog API | On entry; searched on demand | |
| Approval threshold | Policy API | On entry | Passed to `feature.panel-order-summary` |

---

## States

Screen-level states only; component states are in their specs.

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| loading | Entry | Page header shown; regions show their own loading states | Default: Route change |
| error | The draft request can't be created or loaded | Full-width error in the main region: "We couldn't start your request. Try again." with a "Try again" `ui.button` | Default: In-place content change |
| unsaved-changes | User tries to leave with items in the draft | `ui.modal.bundle` asks to keep or discard the draft | None — `ui.modal.bundle` owns its motion |

---

## Responsive behavior

Follows the `sidebar-content` reflow rules in `layouts.md`. On compact, the order summary moves below the catalog and stops being sticky.

---

## Roles and permissions

| Role | Sees | Can do |
|---|---|---|
| Employee | This screen | Create and submit their own requests |
| Operations | This screen | Same as employee; approval happens elsewhere |

---

## Analytics events

| Event | Fires when | Properties |
|---|---|---|
| `request_screen_viewed` | Screen loads | `hasDraft` |

---

## Error handling

| Error | Cause | User sees | Recovery |
|---|---|---|---|
| Draft can't be created or loaded | Request API error or timeout | "We couldn't start your request. Try again." | "Try again" reloads the screen data |
| Submission fails | Request API error or timeout | `ui.toast`: "Your request wasn't submitted. Try again." | The draft is kept; the user can submit again |

---

## Accessibility

- **Page title:** "New request – Stockroom".
- **Focus on arrival:** the page header's "New request" heading.
- **Focus not obscured:** the sticky order summary and the toast never fully hide the focused element in the catalog.

---

## Acceptance criteria

Composition only. Acceptance criteria for `feature.panel-order-summary` and `feature.catalog-picker` apply and are not restated.

- **AC-1:** **Given** an employee with no draft, **when** the screen loads, **then** a new draft MUST be created and the catalog and an empty order summary MUST be shown.
- **AC-2:** **Given** the draft can't be created, **when** the screen loads, **then** the full-width error MUST be shown in place of the catalog and order summary.
- **AC-3:** **Given** items are added in the catalog, **when** quantities change, **then** the order summary MUST reflect them without a page reload.
- **AC-4:** **Given** a compact viewport, **when** the screen renders, **then** the order summary MUST appear below the catalog.
- **AC-5:** **Given** a draft with items, **when** the user tries to leave the screen, **then** the discard confirmation MUST be shown.
- **AC-6:** **Given** a submission fails, **when** the error returns, **then** the toast MUST be shown and the draft MUST be kept unchanged.
