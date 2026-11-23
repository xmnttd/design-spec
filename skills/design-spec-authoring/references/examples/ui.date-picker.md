---
title: ui.date-picker
type: ui-component
status: draft
last-updated: 2026-10-13
---

**Role:** Lets someone choose one date, such as the date they need an item by.

---

## Library

- **Built on:** shadcn/ui Date Picker pattern: `Popover` holding a `Calendar` (`mode="single"`), opened by a `Button` (`variant="outline"`). Docs, at the version in `intent.md`: [Date Picker](https://ui.shadcn.com/docs/components/date-picker), [Calendar](https://ui.shadcn.com/docs/components/calendar), [Popover](https://ui.shadcn.com/docs/components/popover).
- **Property map:** `value` → `Calendar` `selected`; `onChange` → `onSelect`; `disabled` → the trigger `Button`'s `disabled`.
- **Used in this product:** single date only. No range or multiple selection, no time, no dropdown month or year captions. Weeks start on Monday.
- **Deviations:**
  - **Past dates are disabled.** Today and later can be chosen; earlier days are shown but not selectable, and are announced as unavailable.
  - **A "Clear" button** sits below the calendar when a date is set. Selecting it removes the date, closes the popover and returns focus to the trigger.
  - **Trigger label:** "Pick a date" when empty; the chosen date in the `intent` date format (for example "14 Oct 2026") when set.
- **Copied code:** the copied `Calendar` and `Popover` files are edited only for the deviations above. Their animation classes are kept as `Library:` motion.

---

## Usage

- **Use when:** a form needs one date, and seeing the calendar helps (needed-by dates, delivery days).
- **Don't use when:** the date is far in the past or the future (a birth date): use `ui.text-field` with date input.
- **Alternatives:** `ui.text-field` for typed dates.

---

## Properties

Only the properties this product uses; the allowed options are in "Used in this product".

| Property | Type | Default | Purpose |
|---|---|---|---|
| `value` | date or none | none | The chosen date |
| `onChange` | handler | — | Called with the new date, or none when cleared |
| `disabled` | boolean | `false` | The trigger can't be opened |

---

## States

Only states the product adds or changes; the library's own states (open, closed, focused day, selected day) apply as the library defines them.

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | — | Trigger shows "Pick a date" or the chosen date | — |
| open | Trigger activated | Calendar opens on the chosen date's month, or this month | Library: Popover open; exit: Popover close |
| past day | A day before today | Shown, not selectable, announced as unavailable | None — it doesn't change |
| cleared | "Clear" selected | Date removed; popover closes; focus returns to the trigger | Library: Popover close |

---

## Accessibility

Only the deviations; the library's calendar keyboard and ARIA behavior apply unchanged.

- Past days are announced as unavailable, not just shown dimmed.
- "Clear" has the accessible name "Clear date".

---

## Visual

`spec-only`: the library's default look with the project's theme. Overrides only:

- The "Clear" button uses `ui.button` `intent=secondary`, full width, below the calendar, separated by `--space-gap-xs`.
