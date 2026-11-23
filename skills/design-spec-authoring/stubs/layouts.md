---
title: layouts
type: foundation
status: draft
last-updated: YYYY-MM-DD
figma-nodes:                      # [figma]
  file: "[Figma file URL]"
  nodes: ["[grid style or layout frame node IDs]"]
figma-version: "[version or lastModified]"   # [figma]
---

<!--
STUB — LAYOUTS
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [if applicable] keep only when relevant

figma mode:      Figma owns layout values (columns, gutters, margins, breakpoint
                 widths, containers). This file holds the rules Figma can't express.
spec-only mode:  this file's ## Visual section owns layout values. Breakpoint
                 widths, column counts, max content width and z-index are
                 written there as values; gutters and margins reference
                 spacing tokens.
Layout values never go in tokens.md.
Breakpoints table: keep only the column for this mode ([spec-only] Applies
from, [figma] Figma frame or grid style) and delete the other.
-->

**Role:** [One sentence: the overall layout approach.]

---

## Breakpoints

| Name | [spec-only] Applies from | [figma] Figma frame or grid style |
|---|---|---|
| [e.g. compact] | [spec-only: see ## Visual] | [figma: frame or style name] |
| [e.g. medium] | [ ] | [ ] |
| [e.g. wide] | [ ] | [ ] |

---

## Page shells

| Shell | Regions | Use for |
|---|---|---|
| [e.g. sidebar-content] | [side menu, header, main] | [Dashboards and work areas] |
| [e.g. focused] | [header, main] | [Forms and single-task screens] |

---

## Reflow rules

| Region | compact | medium | wide |
|---|---|---|---|
| [e.g. side menu] | [Collapses to a menu button] | [Icons only] | [Full width] |

---

## Scroll, sticky and layering

- **Scroll:** [which region scrolls; whether the page or the main region scrolls.]
- **Sticky:** [which elements stick, and where.]
- **Max width:** [behavior beyond the widest breakpoint.]
- **Layering order:** [from base to top, e.g. content, sticky header, menus and popovers, modal backdrop, modal, toasts.]

---

## Grid by screen type

| Screen type | Shell | Grid |
|---|---|---|
| [e.g. dashboard] | [sidebar-content] | [grid name] |

---

## Visual

[spec-only]

| Breakpoint | Min width | Columns | Gutter | Margin | Max content width |
|---|---|---|---|---|---|
| [name] | [value] | [n] | [spacing token] | [spacing token] | [value] |

| Layer | z-index |
|---|---|
| [e.g. sticky header] | [value] |
