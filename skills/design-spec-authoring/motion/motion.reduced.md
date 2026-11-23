---
title: motion.reduced
type: foundation
status: ready
last-updated: 2026-10-15
---

**Role:** Reduced motion as a second designed state, not a switch that turns motion off. It removes travel while keeping every state change visible.

It applies whenever the person's operating system or browser asks for reduced motion (`prefers-reduced-motion`). If the product also offers its own setting, that setting produces exactly this state. It also applies to library components, unless the Source line in `motion` records that the library's reduced-motion behavior is used.

---

## What changes

- **Travel is removed.** Entrances and exits that move become fades.
- **Fades stay.** Every remaining fade runs at `--motion-duration-fast`. Fades are never set to zero, so the change stays perceivable and completion events still fire.
- **Position and size MAY change instantly** (a toggle indicator, a region's height, a scroll position, an indicator glyph) as long as the change itself is still visible.
- **Sequencing collapses.** Staggers and content-overlap delays become immediate.
- **Anything a style adds is removed.** Scale, springs, overshoot, blur, signatures and optical effects that move. A style's own `motion.reduced` says how.
- **Color and appearance changes are unchanged.**

---

## By default

| Default | Reduced-motion behavior |
|---------|------------------------|
| Hover, Press, Focus, Disabled | Unchanged |
| Selection — toggle | Indicator appears in its new position instantly; colors change as normal; labels crossfade at `fast` as well |
| Disclosure — expand / collapse | Region changes height instantly; contents fade at `fast`; indicator glyph swaps instantly |
| Surface enter / exit — small | Fade only, at `fast`; tooltip dwell unchanged |
| Surface enter / exit — modal | Scrim and dialog fade only, at `fast` |
| Surface enter / exit — panel | Fade only, at `fast`; contents appear with the panel |
| Content entrance | Fade only, at `fast`, all at once |
| In-place content change | Crossfade at `fast` |
| Reposition | Item appears in its new position instantly, with a fade at `fast` so the move is noticed |
| Value change | The fill, bar or needle takes its new value instantly; the number beside it crossfades at `fast` |
| Group reveal / hide | Fade only, at `fast`, all at once |
| Validation error / clear | Message fades at `fast`; border and color unchanged |
| Programmatic scroll | Jumps to the target instantly |
| Route change, Route change — top level | Outgoing and incoming fade at `fast`; no travel |
| Theme change | Unchanged |
| Loading | Skeletons are static at full opacity. Indeterminate indicators keep moving, because they show that work is still happening. Delay and minimum hold are unchanged |
| Hover — lift | Unchanged (per Hover) |
| Drag — pick up | Unchanged; tracking is unchanged (MOT-7) |
| Settle | Takes its resting place instantly on release |
| Container transform | Source and destination crossfade at `fast` |
| Shared element | Old and new copies crossfade at `fast` in place |
| Scroll reveal | None: content is present as it scrolls into view |
| Pop-in / Pop-out | Fade at `fast` |
| Confirmation | The mark fades in at `fast` |
| Fallback | Fade only, at `fast` |

---

## Perceivability

Every state change MUST remain visible in reduced motion through at least one of: a color change, an opacity change, a change of shape or icon, or a change of content. If removing travel would leave a state change with none of these, the spec MUST add one.

---

## No motion

A third level, below reduced, for products that offer a "No motion" setting ("Motion levels" in `motion`). No operating-system preference maps to it.

- **Every change is instant.** No fades, travel or loops.
- **Loading is static.** "Loading…" text replaces skeleton pulses and indeterminate indicators; progress text shows where progress is known.
- **Completion events fire at once.**
- **Perceivability still holds**, as above.
- **Announcements are unchanged** ("Announcements" in `motion`).

---

## Accessibility references

| Criterion | Level | How this page meets it |
|-----------|-------|------------------------|
| WCAG 2.2 — 2.3.3 Animation from Interactions | AAA | Travel triggered by interaction is removed on request |
| WCAG 2.2 — 2.2.2 Pause, Stop, Hide | A | Loops stop when loading completes; a wait expected to exceed five seconds SHOULD show progress text alongside its indicator |
| WCAG 2.2 — 2.3.1 Three Flashes or Below Threshold | A | No default flashes |

Meeting 2.3.3 goes beyond the AA target in `intent`; it is part of this motion system by design.

---

## Spec-level reduced motion

A spec that needs different reduced-motion behavior documents it in its Accessibility section. It still meets the perceivability requirement above.
