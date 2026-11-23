---
title: ui.badge
type: ui-component
status: ready
last-updated: 2026-10-02
---

**Role:** Shows a short, read-only status or category next to the item it describes.

---

## Usage

- **Use when:** labeling the status of an item (a request is "Approved") or a category ("Hardware").
- **Don't use when:** the label is interactive; use `ui.button` or `ui.chip`. Don't use for counts on navigation items; use `ui.count`.
- **Alternatives:** `ui.inline-alert` when the status needs an explanation.

---

## Anatomy

1. **Leading icon:** optional; required for `status=error` and `status=warning`, so meaning isn't carried by color alone.
2. **Label:** required; one or two words, sentence case. Maximum 20 characters; never truncates.

---

## Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `label` | string | — | Visible text |
| `status` | enum: `neutral`, `info`, `success`, `warning`, `error` | `neutral` | Meaning |
| `icon` | icon name | none | Optional leading icon |

---

## Variants

| Variant | Differs by |
|---|---|
| `status=success`, `info`, `warning`, `error` | Color pair from the matching status tokens; `warning` and `error` require a leading icon |

---

## Generative use

- **Available as:** `both`.
- **When to use:** the person asks about the status of one item, such as a request. The badge shows that status in a word or two.
- **Data contract:** `label` (string, within the Anatomy limits; required), `status` (one of the `status` values; required) and `icon` (optional; required for `warning` and `error`). The label and status MUST come from the item's real status, never invented.
- **Variants it may pick:** any `status`, matching the item's real status.
- **When activated:** nothing; the badge isn't interactive.
- **Never:** show a count, stand in for an action, or pair a `status` that doesn't match the label.
- **Worked examples:** `label="Awaiting approval"`, `status=info` → an info badge reading "Awaiting approval". `label="Rejected"`, `status=error`, `icon="error"` → an error badge with its icon, reading "Rejected".

---

## Accessibility

- **Role:** none; rendered as text.
- **Accessible name:** the label. If the badge's meaning depends on context ("Approved" next to a request), the surrounding text or the item's accessible name MUST include it.
- **Color:** meaning is never conveyed by color alone.

---

## Visual

- **Surface:** background `--color-status-{status}-subtle`; border none; radius `--radius-pill`; no elevation.
- **Typography:** `--font-label-sm`; color `--color-status-{status}-on-subtle`.
- **Spacing:** padding block `--space-inset-2xs`, inline `--space-inset-sm`; gap between icon and label `--space-gap-xs`.
- **Icon:** size `--size-icon-sm`; color inherits from the label (`currentColor`).
- **Dimensions:** height `fit-content`; width `fit-content`.
- **Variants:** each `status` swaps only the two status color tokens; everything else is unchanged.
