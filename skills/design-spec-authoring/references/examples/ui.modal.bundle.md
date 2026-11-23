---
title: ui.modal.bundle
type: ui-component
status: ready
last-updated: 2026-10-02
parts: [ui.modal.header, ui.modal.body, ui.modal.footer]
figma-nodes:
  file: "https://www.figma.com/design/EXAMPLEKEY/Stockroom"
  nodes: ["40:1180"]
figma-version: "2214508810"
---

**Role:** A dialog shell for a focused task or confirmation that interrupts the current screen. It provides the frame (title, dismiss control, actions); each use supplies its own content.

---

## Parts

Parts are never used outside the modal.

- **`ui.modal.header`:** the title (required, maximum 60 characters, wraps to two lines, then truncates with an ellipsis) and a close control (`ui.icon-button`, icon `close`, accessible name "Close").
- **`ui.modal.body`:** a slot for any content. Scrolls internally when the modal reaches its maximum height; header and footer stay fixed.
- **`ui.modal.footer`:** optional; holds up to two `ui.button`s. Primary action last. For a confirmation, the actions are a `secondary` "Cancel" and a `primary` (or `danger`, for destructive actions) button whose label names the action ("Delete request", not "OK").

---

## Usage

- **Use when:** the user must complete or explicitly dismiss a short task before continuing, or confirm an action that can't be undone.
- **Don't use when:** the content is long or needs its own URL; use a screen. Don't use for passive messages; use `ui.toast` or `ui.inline-alert`.
- **Alternatives:** `ui.drawer.bundle` for supporting tasks that keep the screen visible.

---

## Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `title` | string | — | Header title and the dialog's accessible name |
| `size` | enum: `sm`, `md` | `md` | `sm` for confirmations; `md` for forms |
| `dismissible` | boolean | `true` | Whether close control, `Escape` and backdrop click dismiss the modal |
| `children` | slot | — | Body content |
| `actions` | slot | none | Footer buttons |
| `onClose` | handler | — | Called on any dismissal |

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | Opened by its trigger; closes when dismissed or its action completes | On open, focus moves into the modal and the screen behind is inert. On close, focus returns to the element that opened it | Default: Surface enter — modal; exit: Surface exit — modal |
| busy | A footer action is loading | Footer buttons follow the `ui.button` loading state; dismissal is blocked until the action finishes | None — `ui.button` owns the loading motion |

---

## Behavior

- Dismissal (when `dismissible`): the close control, `Escape`, or a click on the backdrop. All three call `onClose`.
- When `dismissible` is false (for example, a required decision), the close control is hidden and `Escape` and backdrop clicks do nothing.
- Only one modal is open at a time. A modal MUST NOT open another modal; use a step inside the body instead.

---

## Accessibility

- **Role:** `dialog`, with `aria-modal="true"` and `aria-labelledby` pointing to the title.
- **Keyboard and focus:** follows the WAI-ARIA Authoring Practices "Dialog (Modal)" pattern: focus moves to the first focusable element in the body (or the close control if there is none); for a confirmation of an action that can't be undone, it moves to the least destructive action ("Cancel"). Focus is trapped inside and returns to the trigger on close. Deviations: none.
- **Screen behind:** inert while open (not focusable, hidden from assistive technology).
