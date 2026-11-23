---
title: ui.button
type: ui-component
status: ready
last-updated: 2026-10-02
figma-nodes:
  file: "https://www.figma.com/design/EXAMPLEKEY/Stockroom"
  nodes: ["12:340"]
figma-version: "2214508810"
---

**Role:** Triggers a single action, such as submitting a form, opening a dialog or confirming a choice.

---

## Usage

- **Use when:** the user performs an action on the current screen.
- **Don't use when:** the user navigates to another location; use a link instead. Don't use a button to toggle a setting; use `ui.switch`.
- **Alternatives:** `ui.icon-button` for compact, icon-only actions in toolbars; `ui.link` for navigation.

---

## Anatomy

1. **Leading icon:** optional; from `icons.md`.
2. **Label:** required; one to three words, sentence case, starting with a verb ("Submit request"). Maximum 24 characters; labels never truncate or wrap.
3. **Trailing icon:** optional; used only for disclosure (`chevron-down`) or external links (`external`).

---

## Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `label` | string | — | Visible label and accessible name |
| `intent` | enum: `primary`, `secondary`, `danger` | `secondary` | Visual emphasis and meaning |
| `size` | enum: `sm`, `md` | `md` | Density |
| `leadingIcon` | icon name | none | Optional icon before the label |
| `trailingIcon` | icon name | none | Optional icon after the label |
| `loading` | boolean | `false` | Shows the loading state |
| `disabled` | boolean | `false` | Prevents interaction |
| `type` | enum: `button`, `submit`, `reset` | `button` | Native button type |
| `onPress` | handler | — | Called on activation |

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | — | Reference state | — |
| hover | Pointer over the button | No behavior change | Default: Hover |
| active | Pressed | `onPress` fires on release, not on press | Default: Press |
| focus | Keyboard focus | Focus is visible | Default: Focus |
| disabled | `disabled` is true | Not focusable; `onPress` never fires. The reason is explained in nearby text, never by disabling silently | Default: Disabled |
| loading | `loading` is true | Label stays visible; a spinner replaces the leading icon; `onPress` doesn't fire; width doesn't change; the change is announced as "Loading" | Default: Loading |

---

## Variants

| Variant | Differs by |
|---|---|
| `intent=primary` | Highest emphasis. A screen region MUST NOT contain more than one primary button |
| `intent=danger` | Used only for destructive actions. A destructive action that can't be undone MUST be confirmed with `ui.modal.bundle` |
| `size=sm` | Used only in dense layouts such as table rows and toolbars |

---

## Behavior

- A button MUST NOT fire `onPress` twice from a double click while a previous action is still loading.
- Buttons in a group are ordered with the primary action last (right-most in left-to-right languages).
- `intent=danger`: `Optics: none`.

---

## Accessibility

- **Role:** native `button` element.
- **Accessible name:** the `label`.
- **Keyboard:** follows the WAI-ARIA Authoring Practices "Button" pattern. Deviations: none.
- **Focus:** in document order; focus is always visible.
- **Disabled:** communicated by more than color; disabled buttons are skipped in tab order.
- **Announcements:** loading announces "Loading", then the action's outcome, as the Loading row of "Announcements" in `motion` describes.
