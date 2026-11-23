---
title: motion.defaults
type: foundation
status: ready
last-updated: 2026-10-15
---

**Role:** How every kind of change moves. Steps within a row run in the order MOT-14 describes: "then" waits for the previous step, "with" starts together. Specs name a default in the Motion column of their States table, for example `Default: Surface enter — small`. Token names below omit the `--motion-duration-`, `--motion-ease-` and `--motion-distance-` prefixes.

Baseline 1.3 includes every default a style can use, at neutral behavior: rows marked *(vocabulary)* behave like the baseline defaults they stand in for, until a style gives them its own.

---

## Classifying a change

When a change could match more than one default:

- A surface **anchored to a trigger** (menu, popover, tooltip, toast) is small. A surface **centered over a scrim** is a modal. A surface **attached to a screen edge** that travels its own width or height is a panel.
- If it **overlays** other content, it is a surface. If it **pushes** other content, it is a disclosure. An element inserted into the layout, such as an inline alert or an error row, is a disclosure.
- If the element stays and its **content** changes, it is an in-place content change. If new content **arrives** in a newly revealed area, it is a content entrance.
- If the **whole screen** below the persistent shell changes, it is a route change. Moving between top-level destinations (the navigation bar, the app's main sections) is **top level**. Drilling into a screen, or going back out of it, is a plain **Route change**.
- If a **value** shown as a length, height or angle changes (a progress fill, a chart bar, a gauge), it is a value change. The number beside it changes per In-place content change.
- If a **persistent element turns into** the surface it opens (a card into its detail, a button into a menu), it is a container transform. If the surface is separate and merely grows from the element, it is Surface enter — small.
- If one element is shown **in both states** and moves or resizes between them, it is a shared element. Inside a single control (a toggle thumb, a tab pill), it is Selection — toggle.
- If content appears **because it scrolled into view**, it is a scroll reveal.
- If a small element appears **attached to another** (on its corner, beside its label) and is not content, it is a pop-in. A new row in a list is a content entrance.
- If the change **acknowledges** that an action finished, it is a confirmation. If it only shows new data, it is an in-place content change.
- If a large pointer target **opens something** (a card, a tile), hovering it is Hover — lift. Every other target uses Hover.
- The moment a **drag begins** is a pick up. The motion **after the person lets go** is a settle.

---

## Tiers

Every change is everyday or a moment ("Tiers" in `motion`). Baseline has one form for both. Confirmation, Container transform, Shared element, Scroll reveal and Theme change are always moments; a spec marks any other moment with `(moment)`. Baseline ignores the mark, but a style applied later uses it.

---

## Defaults

| Default | Applies to | What changes | Duration | Easing | Distance / delay |
|---------|-----------|--------------|----------|--------|------------------|
| Hover | Any pointer target | Background color, color, border color, outline color, shadow | `fast` | `state` | — |
| Press | Buttons, tappable items | Same properties as Hover | `fast` | `state` | — |
| Focus | Anything focusable | Focus indicator appears immediately (MOT-3) | — | — | — |
| Disabled | Any control | Applies immediately | — | — | — |
| Selection — toggle | Switch, checkbox, radio, tabs, segmented control, chip | Indicator (thumb, mark, pill) moves to its new position as one persistent element, *with* colors changing and labels crossfading | `fast` | `state` | Indicator's own track |
| Disclosure — expand | Accordion, collapsible section, expandable row | Region grows to fit its content, *with* the indicator glyph turning or swapping (`fast`, `state`); *then* contents arrive per Content entrance | `medium` | `move` | — |
| Disclosure — collapse | Same | Contents fade out (`fast`, `exit`) *with* the region shrinking | `medium` | `move` | — |
| Surface enter — small | Menu, dropdown list, popover, toast, tooltip | Fades in and travels away from its anchor or edge. Tooltips fade without travel and wait `dwell` first | `medium` | `enter` | `ui`; tooltip `dwell` |
| Surface exit — small | Same | Fades out | `fast` | `exit` | — |
| Surface enter — modal | Modal, dialog, and its scrim | Scrim fades in *with* the dialog fading in and rising; *then* contents arrive per Content entrance | `medium` | `enter` | `ui` |
| Surface exit — modal | Same | Scrim fades out *with* the dialog | `fast` | `exit` | — |
| Surface enter — panel | Side panel, drawer, sheet | Travels in from its edge across its own width or height; contents arrive per Content entrance, starting `content-overlap` after the travel starts | `crawl` | `canvas` | Own dimension; `content-overlap` |
| Surface exit — panel | Same | Contents fade out (`fast`, `exit`); *then* the panel travels out to its edge, *with* the surrounding layout moving alongside it | `crawl` | `canvas` | Own dimension |
| Content entrance | Content arriving in a newly revealed area, including new messages in a conversation | Fades in and rises into place from `text` below its final position. Headings arrive first; *then* each following item arrives `stagger-item` after the one before, for at most three steps; anything after the third arrives with the third | `medium` | `enter` | `text`; `stagger-item` |
| In-place content change | Text and value updates, tab panel swaps, skeleton to content, poster to scene | Outgoing content fades out (`fast`, `exit`); *then* incoming content fades in without travel | `medium` | `enter` | — |
| Reposition | An item moving within a list or layout, reordering | Moves to its new position | `slow` | `move` | — |
| Value change | Progress and meter fills, chart bars, gauges | The fill, bar or needle moves from its old value to its new one. On first appearance, values arrive with their content per In-place content change rather than growing from zero | `slow` | `move` | — |
| Group reveal | A set of controls appearing with a mode change | Each fades in and travels laterally, offset by `stagger-item` | `crawl` | `canvas` | `ui`; `stagger-item` |
| Group hide | Same | All fade out together | `fast` | `exit` | — |
| Validation error | Fields and controls rejecting input | Border and color change to the error state (`fast`, `state`) *with* the message fading in | `medium` | `enter` | — |
| Validation clear | Same | Error state and message fade out | `fast` | `exit` | — |
| Programmatic scroll | The product scrolls a region to a target | Scroll position moves to the target. Scrolling by the person is native (`Platform: native scrolling`) | `medium` | `move` | — |
| Route change | Drilling into a screen, or going back out of it | Outgoing content fades out (`fast`, `exit`); *then* incoming content fades in and travels horizontally by `text`: from the inline end going forward, from the inline start going back. The persistent shell (header, navigation) doesn't move | `crawl` | `canvas` | `text` |
| Route change — top level | Moving between top-level destinations | Fade through: outgoing content fades out (`fast`, `exit`); *then* incoming content fades in without travel. The persistent shell doesn't move; the navigation indicator follows Selection — toggle | `medium` | `enter` | — |
| Theme change | Switching light and dark, or any theme | Color properties crossfade; nothing moves | `fast` | `steady` | — |
| Loading | Any wait, inline or screen-level | Nothing shows for `delay-loading`; once shown, the indicator stays at least `hold-min`. Skeletons pulse opacity between full and 60% at `crawl` per half-cycle; indeterminate indicators complete one cycle per two `crawl` steps. Content replaces the indicator per In-place content change | `crawl` | `steady` | `delay-loading`; `hold-min` |
| Hover — lift *(vocabulary)* | Cards, tiles and other large pointer targets that open something | Per Hover | `fast` | `state` | — |
| Drag — pick up *(vocabulary)* | A card, list row or tile as a drag begins | None: the element starts tracking the pointer (MOT-7) | — | — | — |
| Settle *(vocabulary)* | A dropped item, a sheet snapping to a stop, a swiped row returning | Moves to its resting place | `medium` | `move` | — |
| Container transform *(vocabulary)* | A card, list item or button that opens into its detail, menu or dialog | Source content fades out (`fast`, `exit`); *then* the destination enters per Surface enter — modal, or per Route change when it fills the screen. Closing runs Surface exit — modal | `medium` | `enter` | `ui` |
| Shared element *(vocabulary)* | A thumbnail that becomes a hero, an avatar that moves into a header, an underline moving between items | The old and new copies crossfade per In-place content change | `medium` | `enter` | — |
| Scroll reveal *(vocabulary)* | Sections and feature blocks on long-form and marketing pages | None: the content is already there | — | — | — |
| Pop-in *(vocabulary)* | Badge, count, unread dot, newly added chip | Fades in | `fast` | `state` | — |
| Pop-out *(vocabulary)* | Same | Fades out | `fast` | `exit` | — |
| Confirmation *(vocabulary)* | Success check, filled like, saved mark | The mark replaces what was there per In-place content change | `medium` | `enter` | — |
| **Fallback** | Anything not listed | Fades, or fades and travels `ui` | `medium` | `state` | `ui` |

---

## Notes

- **Hover-only content** on dense UI (actions revealed on a table row) waits `delay-hover-intent`, then fades in per Hover.
- **Toasts** enter from the screen edge they are anchored to and exit per Surface exit — small, whether dismissed or timed out.
- **Tabs** combine two defaults: `Default: Selection — toggle + In-place content change`.
- **Programmatic scroll and Disclosure — expand** share `medium` and `move`. When an expand also scrolls the region, both run together as one movement.
- **Retargeting (MOT-5).** Rows that name a spring token may redirect from their current position when interrupted, if the element stays on screen. In baseline the spring tokens are tweens, so every interruption jumps to rest.
