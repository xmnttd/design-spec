---
title: motion
type: foundation
status: ready
last-updated: 2026-10-15
baseline: "1.3"
style: none
pages: [motion, motion.tokens, motion.defaults, motion.reduced]
---

**Role:** How this product moves. Every kind of state change has a default here, ready to build from. When the product's motion is customized, the designer updates these pages; specs pick up the change automatically, because they reference names rather than values.

These pages own all motion values in both `figma` and `spec-only` mode.

**Source:** baseline 1.3. No style or component library's motion is in use.

---

## Pages

| Page | Read when |
|------|-----------|
| `motion` | You need the rules, or how specs reference motion |
| `motion.tokens` | You need a token name or value |
| `motion.defaults` | You are filling the Motion column of a States table |
| `motion.reduced` | You are specifying or building reduced-motion or no-motion behavior |
| `motion.optics` | Only when an applied style ships one: its optical effects |

---

## How specs reference motion

ui, feature and scene components, and screens, carry a **Motion** column in their States table. Each row names how the change into that state moves:

| Value | Use |
|-------|-----|
| `Default: <name>` | A default from `motion.defaults` |
| `Default: <name> (moment)` | A default that is a moment here rather than everyday (see "Tiers"), such as `Default: Content entrance (moment)` on an onboarding screen |
| `Default: <name> + <name>` | Two defaults that run together on one change, such as `Default: Selection — toggle + In-place content change` for tabs |
| `Platform: <what>` | The motion belongs to the operating system or browser, such as the on-screen keyboard or native scrolling |
| `Library: <what>` | The motion is a built-in animation of a library component, used as the library defines it (see "Using a component library") |
| `None — <reason>` | The change has no motion, as a decision |
| `—` | The reference state (`default`), when it has no motion of its own |

- **The reference row names motion when the component has its own.** A component that appears or disappears, loops, or shows a changing value names that motion on its `default` row: a skeleton names `Default: Loading`, a progress bar `Default: Value change`, an inline alert `Default: Disclosure — expand; exit: Disclosure — collapse`, a modal `Default: Surface enter — modal; exit: Surface exit — modal`.
- **Variants switched while the product is in use are state changes.** A variant the person changes at runtime (checked, selected, expanded, on/off) names its motion in its "Differs by" cell, for example `checked=on` · `Default: Selection — toggle`. A variant fixed when the component is placed (size, shape, status) names motion only where it differs.
- Leaving a state reverses its entry. Where a default has a paired exit (Surface exit, Disclosure — collapse, Group hide, Validation clear) and the table has no row for the exit, the cell names both: `Default: Surface enter — small; exit: Surface exit — small`.
- Bundles keep one States table; rows for parts name the part.
- Screens cover screen-level states only. Arriving on the screen follows the Route change default that matches how the person got there, and isn't repeated. A screen state that is reached only by arriving names that default (`Default: Route change` or `Default: Route change — top level`), never `None`.
- Scenes cover changes visible around the scene, such as the poster giving way to the scene. Animation inside the scene belongs to its asset: `None — the asset owns this animation`.
- Flows carry no Motion column. Moving between steps follows Route change; a step that moves differently names its default in the System response cell.
- Native scrolling and the on-screen keyboard are `Platform:` values, not `None`. A library component's own animation is a `Library:` value.
- Cells hold names only, never values.

---

## Exceptions

A spec that departs from the default it names describes the exception in its Behavior section: what it overrides, its reduced-motion behavior, and how it behaves when interrupted. The exception applies to that spec only and is never generalized to others.

---

## Rules

These rules apply to every spec and are verified here, once. Specs don't restate them as acceptance criteria; a spec writes a Given/When/Then criterion only for motion behavior it owns, such as an exception.

| ID | Level | Rule |
|----|-------|------|
| MOT-1 | MUST | Every duration, easing, distance and delay references a token in `motion.tokens`. |
| MOT-2 | MUST | Every state change behaves as `motion.reduced` describes when reduced motion is set, unless the spec documents its own reduced-motion behavior in its Accessibility section. |
| MOT-3 | MUST | Focus indicators appear immediately, with no transition. |
| MOT-4 | MUST NOT | Motion is never the only signal that a state changed. A change in color, opacity, shape or content is also visible. |
| MOT-5 | MUST | New input wins. A running animation jumps to its finished state and the new interaction starts from there. Nothing queues. **Retargeting:** an element that stays on screen and moves on a spring token (a toggle thumb or selection indicator, a shared element, an item being reordered) MAY instead redirect to its new target from its current position and velocity, without a jump. It still never queues, and it ends exactly at rest (MOT-12). |
| MOT-6 | SHOULD NOT | Motion does not block input. A spec MAY document a brief lock where a double activation would corrupt state. |
| MOT-7 | MUST | Direct manipulation (dragging, scrubbing, panning) tracks the input with no duration or easing. Easing applies only after release. |
| MOT-8 | SHOULD | Prefer opacity, transform and color changes, which are cheap to animate. A spec that animates the size or position of surrounding layout says so in its Behavior section. |
| MOT-9 | MUST | Loading indicators don't appear for waits shorter than `--motion-delay-loading`, and stay visible for at least `--motion-hold-min` once shown. |
| MOT-10 | MAY | Motion tokens MAY be mirrored as Figma variables for prototyping. If they are, `motion.tokens` records the mirror in its front matter (see "Figma mirror" there), names match 1:1, and these pages win where they differ. |
| MOT-11 | MUST | **Before.** An entering element is not visible before its entrance starts, including while it waits for a delay or its turn in a stagger. A leaving element stays fully visible until its exit starts. |
| MOT-12 | MUST | **After.** Every animation, including an interrupted one (MOT-5), ends with the element in its resting state: exactly as it appears with motion turned off. Nothing is left faded, shifted, scaled or clipped. |
| MOT-13 | MUST NOT | **Space.** Motion never cuts content off. Where a container would clip content that travels, scales or overshoots, that content fades without moving instead. Text is never clipped at any point. |
| MOT-14 | MUST | **Order.** The steps of a default start either *then* (when the previous step finishes) or *with* (at the same moment). Outgoing content finishes before incoming content starts, unless the default says *with*. |

Rule IDs use the `MOT-` prefix so they can't be confused with other systems' names.

These rules hold for every style: the baseline, a remapped library style, or a replacement. Overriding changes values and defaults, never these rules. MOT-12 is checked by comparing a screen after its motion finishes with the same screen rendered with motion turned off; the two match.

---

## Styles

A predesigned **style** (expressive, bouncy) can replace the values in `motion.tokens` and the behaviors in `motion.defaults` and `motion.reduced`, and may add a `motion.optics` page. It never changes names, rules MOT-1 to MOT-14, the system-wide rules below, or this page beyond its front matter and Source line. The style in use is recorded in the front matter (`style`, and `optics: on` or `off` when the style ships optics) and in the Source line.

Every token and default a style can use already exists in this baseline at a neutral value, so applying, switching or removing a style never adds or removes a name.

---

## System-wide rules

These hold for baseline and for every style.

### Overshoot policy

Baseline never overshoots. A style that does follows these limits:

1. **Overshoot lives only in spring tokens.** Role tokens (`state`, `enter`, `exit`, `move`, `canvas`, `steady`, `fade`) never overshoot in any style. A default overshoots only where its row names a `spring-*` token, so a spec exception that names `move` is always safe.
2. **Effects never overshoot.** Opacity, color and blur use `fade` (or the step's own easing), never a spring. Opacity past full and color past its target are meaningless.
3. **Exits never overshoot.** Leaving is quick and accelerates out.
4. **Values never overshoot.** A fill, bar or needle that passes its value misstates it for a moment.
5. **Edges never overshoot.** Panels, drawers and sheets attached to a screen edge would detach from it. They use `canvas`.
6. **Layout never overshoots.** Region height (disclosure), scroll position and anything that pushes surrounding content move without overshoot, so neighbors don't jolt.
7. **Clipping wins.** Under MOT-13, content inside a container that would clip its overshoot fades without moving instead.
8. **Blur is an effect.** It runs with the opacity it accompanies, on `fade` or the step's own effects timing, never on a spring. It applies only to text, icons, marks and elements no larger than a dialog; screens, panels, scrims, images and video never blur (MOT-8). It ends at exactly 0, with the filter removed, at rest (MOT-12), and it is removed in reduced motion.
9. **Gestures carry velocity, not overshoot rules.** A spring started by a release (Settle) MAY begin at the release velocity, and that velocity MAY carry it past rest. The limits above still apply: an edge-attached sheet settles without overshoot whatever the release speed.
### The hand

Motion the person's hand drives must answer immediately. Press release, Hover — lift, Selection — toggle and Drag — pick up run on `spring-hand` or on a tween no longer than 150ms, and their motion is within 2% of rest by 150ms in every style. A style's character on these rows comes from shape (a quick single overshoot, a critically damped glide), never from length. Settle after a drag is a landing, not the hand, and is exempt.
### Tiers: everyday and moments

Every change is either **everyday** or a **moment**.

- **Everyday** changes happen many times a session: hovers, presses, toggles, menus, validation, reordering, most navigation. They stay quick and quiet in every style.
- **Moments** happen rarely and matter: a completed purchase, a first arrival, onboarding, a card opening into its detail. A style's character is concentrated here: full cascades, larger travel, emphasis, and its optical effects.

**Always moments:** Confirmation, Container transform, Shared element, Scroll reveal, Theme change.
**Everything else is everyday** unless a spec marks it as a moment, by adding `(moment)` to the Motion value: `Default: Content entrance (moment)` for an onboarding screen, `Default: Surface enter — modal (moment)` for an upgrade dialog. Marking a moment changes no tokens and no names; each style's `motion.defaults` says what its moments add (its "Tiers" section).

**A moment seen again is everyday.** The second time the same moment happens in a session, and every repeat that interrupts the previous change (MOT-5), it runs in its everyday form.

Baseline has one form for both tiers; a spec MAY still mark moments, so they carry the style if one is applied later.
### Duration and size

The row's role chooses the duration token; size can step it up. When an element travels or scales much further than its role usually does (more than about a third of the viewport, or a surface wider than about 600px), it uses the next token up: `fast` → `medium` → `slow` → `crawl`, or `spring-fast` → `spring-default` → `spring-slow`. Never beyond `crawl` or `spring-slow`, never for the hand, and never down: a small change keeps its row's token. Larger things take a little longer, so they read as heavier rather than faster.
### Motion levels

Three levels apply over whichever style is active.

| Level | Set by | Behavior |
|---|---|---|
| Full | Default | The style as written |
| Reduced | `prefers-reduced-motion: reduce`, or the product's setting | `motion.reduced`: travel, scale, springs, blur, cascades, optics that move and signatures removed; fades at `fast` |
| None | The product's "No motion" setting only (no operating-system preference maps to it) | Every change is instant: no fades, no loops. Loading shows static text ("Loading…") and progress text where progress is known. Completion events fire at once. Every state change still meets the perceivability rule in `motion.reduced` through color, shape, icon or content |

A product that offers motion settings offers all three: Full, Reduced, None.

**Signatures.** A style's most distinctive, least essential effects are **signatures**, listed in its `style.md`: expressive's theme reveal, bouncy's validation shake. Each can be switched off on its own in `motion` front matter (`signatures: { theme-reveal: off }`), falling back to the row's plain form. Reduced and None remove them.
### Device tiers

Motion steps down on devices that can't keep up, independently of the person's motion level. The product picks the tier at start and MAY step down during a session; it never steps up mid-session.

| Tier | When | Removes, in this order |
|---|---|---|
| Full | Default | Nothing |
| Lite | Low-end hardware (about 4 GB of memory or less, 4 cores or fewer), `prefers-reduced-data` or a data-saver mode, a low-power mode, or frames dropping below about 50fps during a change | Optics; then blur; then cascades collapse to a single step; then physics springs run as their CSS curves (no velocity) |
| Minimal | Very low-end hardware, or frames still dropping on Lite | Everything `motion.reduced` removes. Fades stay |

A tier never removes a state change, only how it moves. Small screens are not a tier: subtle motion is easier to miss there, so a mobile layout keeps the full tier when the device can carry it.
### Announcements

Motion is invisible to screen readers. Changes the person didn't directly cause, or whose result isn't where their focus is, are also announced through a live region. Announcements belong to the change, not the motion, so they are identical in every style, level and tier.

| Default | Announce | Live region |
|---|---|---|
| Confirmation | The outcome ("Payment complete") | `status` (polite) |
| Validation error | The error message, when it appears after a submit or on blur | `alert` (assertive) for errors that block submitting; `status` otherwise |
| Pop-in | The new count or state ("3 new notifications") | `status` |
| Toast (Surface enter — small) | Its text | `status`; `alert` only for urgent failures |
| Loading | "Loading" once, after `delay-loading`; then the completion ("Results loaded") | `status` |
| In-place content change | The new value, when it changes without the person acting (a live price, a status) | `status` |
| Reposition | The new position, when the person moved it by keyboard ("Moved to position 1 of 4") | `status` |
| Route change | Nothing extra: focus moves to the new screen's heading | — |

- **At the change, not after the motion.** The announcement is queued the moment the state changes. It never waits for a cascade or spring to finish.
- **Once per change.** A cascade of items is one announcement ("4 new messages"), never one per item.
- **Nothing decorative.** Optics, signatures and transitions themselves are never announced.
### Optics layer

Optics are transient light effects: glints (a light sweep across a surface), glows (a soft halo that rises and fades), edge glows (a glow along an element's outline only, never its face) and shimmer (a highlight band crossing loading text). They are drawn with gradients and shadows over the motion that a default already defines.

A style MAY ship a `motion.optics` page. It is laid out like `motion.reduced`: one table, one row per default that gains an effect, plus the tokens the effects use. A style without the page has no optics, and switching it on does nothing.

**Why it can be a switch.** Optics only add light. Every optical effect obeys these seven rules, so turning the whole layer on or off at any moment changes nothing else:

1. **Never the signal.** Every state change is complete without its effect (MOT-4). A glow on an error field is in addition to its border, color and message.
2. **In time with the motion, never changing it.** An effect is timed against the steps of the change it decorates, never by a free-standing delay. By default it starts on the first frame of its step, with no wait, and peaks as that motion completes 90% of its travel, so light and movement arrive together; it then decays on `fade` over `crawl`. It MAY instead wait for a later moment that the motion itself defines when it decorates that later step, anchored to a step's 90% point rather than its nominal end, because an eased step looks finished before its duration is up: a glint that should cross a control only once its new color has settled starts at the color change's 90% point. A delayed effect still starts before the change finishes, and lasts at least `fast`. An effect on a held state (a hover lift) runs on the motion's own duration and easing, rising and falling exactly with it. Effects never delay, extend or reorder the motion; removing them removes nothing else.
3. **Never the layout.** Effects are overlays and shadows. They don't move, resize or reflow anything, and they are clipped to their element or drawn as its shadow.
4. **Gone at rest.** Every effect returns to nothing when its change finishes (MOT-12). Shimmer is the one exception: it lasts exactly as long as the loading it marks.
5. **Moments and held states only.** An effect decorates either a moment (tiers, above) or a held state that lasts while the person holds it: a hovered card's lift, a pressed control, a wait while loading. Everyday changes carry no optics, so frequent interactions stay quiet.
6. **Small and few.** Effects go on controls, marks, badges, cards and surfaces up to dialog size, never on screens, images or video. At most one effect per change.
7. **Light, not blur.** Effects are gradients and shadows. Blur stays in `motion.tokens` (`blur-enter`, `blur-exit`).

**How it differs from reduced motion.** Reduced motion is an accessibility state the person asks for, and it applies to every style. Optics are an optional design layer that the product decides to ship. Both apply on top of whichever style is active, and both are switched by a single setting.

**On, off, and who decides.** The product decides whether the layer ships, recorded in `motion` front matter as `optics: on` or `optics: off`. A style that ships optics has them **on by default**; `optics: off` turns the layer off for the product. When it ships, it turns off by itself when:

- the person asks for reduced transparency (`prefers-reduced-transparency: reduce`), higher contrast (`prefers-contrast: more`) or forced colors (`forced-colors: active`);
- the device is in a low-power mode;
- the product offers its own "No optical effects" setting and the person turns it on (it works like "No motion": a switch that removes, not one that adds).

**With reduced motion.** Effects that move (glint, shimmer) are removed. Glows and edge glows stay, as fades at `--motion-duration-fast`, because they are a change of light, not of position.

**In specs.** Specs never reference optics tokens and never depend on an effect. A component that should never carry an effect (a dense table row, a destructive button) says `Optics: none` in its Behavior section.

---

## Relationship to intent

`intent` holds the project-wide reduced-motion rule and the accessibility target, and names which kind of loading indicator is used. These pages own all motion timing, including loading delays.

---

## When nothing fits

1. Classify the change using the tie-breakers in `motion.defaults`.
2. Name the matching default.
3. If no default matches, name `Default: Fallback`.

Never invent a value or token, and never leave a cell empty. A change that uses the Fallback is a candidate for a new default in `motion.defaults`.

---

## Using a component library

When `intent` names a component library (`components.source` is not `custom`), its motion is brought in only as far as the designer chooses. The designer is asked once, when the library is adopted, which parts to use. Each part lands in one place:

| Part of the library's motion | Where it lands | Recommended |
|---|---|---|
| Timing values (durations, easing curves) | Values in `motion.tokens`; token names stay as they are | Use |
| Transition patterns (how screens and containers change) | Rows in `motion.defaults`, remapped or added, each with its row in `motion.reduced` | Use |
| Built-in component motion (the library components' own animation) | `Library: <what>` in the Motion column of library-backed specs | Use, for library components |
| Reduced-motion behavior | `motion.reduced` | Keep this system's |
| Springs (physics-based motion) | Values of the spring tokens in `motion.tokens`, and defaults that use them | Only for deliberately expressive motion. The expressive and bouncy styles provide springs ready-made |

- **Confirm before editing.** If the answer is general ("use the library's motion"), the recommended answers apply. The changes are listed for the designer before any page is edited.
- **Record the answer.** The Source line at the top of this page names the library and its version (from `intent`), and which parts were used and which were kept.
- **Library components move as the library defines,** for the parts that were used. Where built-in component motion is not used, library components follow `motion.defaults`. That choice is made here once and is not restated as a deviation in each spec.
- **Custom components** in a set that mixes library and custom components follow `motion.defaults`. Remapping the defaults to the library's patterns is how they come to match the library components beside them.
- **What never changes:** default and token names, and rules MOT-1 to MOT-14.

---

## Changing this system

- **Values** change in `motion.tokens`; **behaviors** change in `motion.defaults`. Specs never change when either does.
- **Every default has a row in `motion.reduced`.** A default is added, changed or removed together with its reduced-motion row.
- **Names are stable.** Keep a default's or token's name and change what it does. To replace a name, add the new one, move every spec to it, and only then remove the old one. Engineering code references token names, so the same applies to tokens.
- Update `last-updated` on every page you change.
