# Page Anatomy

What the preview contains and how each page is laid out. Applies in both modes; where the content comes from is in `figma-mode.md` and `spec-only-rendering.md`.

---

## Contents

1. Shell and navigation
2. Page header (every page)
3. Overview
4. Foundations
5. Components (ui, feature, scene)
6. Screens
7. Flows
8. Spec content
9. Chrome and style rules
10. Inline helpers

---

## 1. Shell and navigation

One `index.html`: a fixed sidebar and a scrolling content area. Below 768px the sidebar collapses to a top bar. One page is shown at a time; inactive pages carry the `hidden` attribute. The URL hash selects the page, so pages can be linked and the back button works.

**Navigation accessibility:** the sidebar is a `<nav>` and the content area a `<main>`; a skip link to `<main>` comes first; the current page's nav link has `aria-current="page"`. On a page change, move focus to the new page's heading (`tabindex="-1"`) and update `document.title` to the page name.

```
Overview
Foundations
  Color · Typography · Spacing · Elevation · Shape and size
  Icons            (if icons.md exists)
  Layouts          (if layouts.md exists)
  Motion           (if the motion pages exist in design-system/motion/)
Components
  UI               ui.<name>, one per spec; bundles show their parts as sub-items
  Feature          feature.<name>, one per spec; bundles show their parts as sub-items
  Scene            scene.<name>, one per spec; bundles show their parts as sub-items
Screens            screen.<name>
Flows              flow.<name>
```

Groups with no specs are left out. Foundations labels are categories; every other nav label is a spec name. No gap report page.

**Motion-level control:** a group of `aria-pressed` buttons labeled "Motion" (Full · Reduced · None) in the sidebar (top bar below 768px). It sets `data-motion="full"`, `"reduced"` or `"none"` on the content area, never on the chrome. Reduced plays the `motion.reduced` state; None plays the project's "No motion" level (every change instant, loading as static text), and is shown only when `motion.reduced` has a "No motion" section (baseline 1.3 or later). It starts on Reduced when the viewer has `prefers-reduced-motion` set, otherwise Full, and persists across pages during the session. Every motion in the preview runs through the motion helper (section 10), which reads `data-motion`, so the control and the system setting behave the same.

**Theme or brand switcher:** if tokens have modes, a native `<select>` with a label at the top of the sidebar (in the top bar below 768px). It sets `data-theme` / `data-brand` on the content area only, never on the chrome. It starts from the first mode, and the choice persists across pages during the session. Switching plays the Theme change default through the motion helper, if the motion pages exist.

---

## 2. Page header (every page)

- Spec name as the page title, and the spec's Role line (or summary) beneath it. Foundation pages split from one spec (Color, Typography… from `tokens`) use the category as the title and name the source spec beneath it ("from `tokens`").
- A metadata row: `status`, `last-updated`, and, in `figma` mode, Ready for dev (from Figma), the Figma node ID(s) and whether `figma-version` matches the current file ("unknown" if either version couldn't be read). During a migration, the visual source ("Figma" or "spec").
- **Buildable** badge, by the rule in `../design-spec-authoring/references/source-of-truth.md`, "Ready for dev and buildable" (flows: status only; library-backed kit components: the override node if there is one, otherwise status). "Not buildable yet" names the reason (status, or Ready for dev). If Ready for dev couldn't be read, show "Unknown: couldn't read Ready for dev" with the neutral mark (section 9) rather than guessing either way. Foundations: no badge.

---

## 3. Overview

- Project name (`intent.md` `project`), the intent summary, and the visual source.
- Counts by nav group (Foundations: icons, layouts and motion where they exist; UI, Feature, Scene; Screens; Flows), and how many specs are buildable.
- A table of every spec in scope: name, type, status, buildable, and links to its page. Deprecated specs appear here only, with their replacement.
- The Figma file(s) and version read (`figma` mode), the date of the preview, and a line saying gaps are reported in chat.
- A short key: what hotpink means, what the neutral mark means, and that forced states are labeled. If the set is library-backed, one informational line: library components are approximated, not the library's own code. If the set has scene components, one informational line: scene content (3D, video, interactive animation) isn't rendered; each scene shows its poster and live chrome.
- One line saying the WCAG 2.2 AA claim (section 9) covers the preview's chrome only, not the design being previewed.

---

## 4. Foundations

Show each token rendered with its own custom property, and its name. In `spec-only` mode, every token in `tokens.md`, with its value. In `figma` mode, what was read (`figma-mode.md`, section 4): the collections `tokens.md` lists, plus the styles the read nodes use, each with its Figma variable path and the resolved value for the current mode. One line says styles the read nodes don't use may not appear.

| Page | Content |
|---|---|
| Color | Swatches, primitives first, then semantics with the primitive each maps to. Mode switching changes the swatches. Text-and-surface pairs that the specs use show their contrast ratio |
| Typography | A live specimen per style or role, with size, line height, weight, letter spacing and family below. Font source noted; fallbacks marked. Styles follow the text-style rule below |
| Spacing | A bar per token, sized by the token, ascending |
| Elevation | A card per shadow token showing the shadow |
| Shape and size | Radii on a sample box; border widths; control heights and icon sizes at their real size |
| Icons | Every icon in `icons.md` (or the Figma icon library frame) at its approved sizes, with name and approved usage, grouped as `icons.md` groups them |
| Layouts | Breakpoints as a labeled ruler; page shells as labeled box diagrams built with real CSS grid and flex; the grid per screen type. A width control resizes the diagrams to show reflow rules (container queries, section 6) |
| Motion | Only if the motion pages exist. The showcase of the motion helper (section 10). The tokens from `motion.tokens` with their values (durations and easings each on a moving sample; spring easings at their paired spring durations; scale, blur and gesture tokens with their values, which at baseline's neutral values look like the tokens they stand in for). Then one live demo per row of `motion.defaults`, with a Play control, its tokens listed, and its `motion.reduced` behavior as text beside it. Each demo follows the motion-level control. Changes in `motion/` (a customized default) show here as written |

**Text-style rule (both modes).** Text styles in `figma` mode, and composite type tokens in `spec-only` mode, become one custom property per style, named after the style, using the `font` shorthand (weight, size/line-height, family), plus a `letter-spacing` property where set. This is the one text-style rule.

---

## 5. Components (ui, feature, scene)

Each component page, in this order:

1. **Interactive demo.** A working instance, stamped from the component's template (section 10), that responds to real input. Required by kind:
   - Buttons and clickable elements: hover, press, keyboard focus with `Tab`, activation with `Enter` / `Space`.
   - Inputs: live typing, focus, validation as the spec defines it (when it runs, what message shows).
   - Checkboxes, switches, radios: toggle by click and `Space`.
   - Menus, selects, comboboxes: open, close, arrow-key navigation, `Escape`, focus return, per the WAI-ARIA pattern the spec names.
   - Dialogs and overlays: a trigger to open; close button, `Escape` and focus return.
   - Data-driven components: controls to switch between the data states the spec names (for example empty, loading, error, populated) using the spec's placeholder data. Demo only states and properties the spec defines.
   - Display-only components: a static instance, labeled "Display component, no interaction."
   The demo never uses forced-state classes.
   - **Motion:** each change into a state calls the motion helper (section 10) with that state's Motion cell; components never re-implement choreography. `Default: <a> + <b>` and `; exit:` pairs are passed as written; a `(moment)` mark is passed with the name and shown in the state's label. `Platform: <what>` (native scrolling, the on-screen keyboard) is left to the browser and labelled "Platform motion". `Library: <what>` (a library component's built-in animation) maps to the default the `motion` page's Source line records for it, labelled "Library motion, approximated"; with no recorded default, use Fallback and mark it with the hotpink "guessed" label. `None — <reason>` changes instantly; `—` is a reference state with no motion of its own. A reference (`default`) row that names a default plays it when the component appears (a modal entering, a skeleton pulsing, a progress bar's value change). A variant switched at runtime moves as its "Differs by" cell names; if that cell names no motion, use the closest matching default and mark it "guessed". Under Reduced, the default's `motion.reduced` behavior applies, unless the spec's Accessibility section describes its own reduced-motion behavior: then that is passed to the helper. Under None, every change is instant. Build only what the default describes; never invent extra movement. Focus always appears immediately. **Announcements:** where the spec's Accessibility section says what a state announces, the demo writes that text to a visually hidden live region (`status` or `alert`, as `motion`'s "Announcements" table says) at the moment the state changes, at every motion level. `Optics: none` needs nothing: the baseline has no optical effects. An exception described in the spec's Behavior section isn't animated: use the named default and label it "Exception described in the spec; shown as the named default".
2. **States.** Every state the spec names, side by side and labeled with its name and its Motion cell (for example "hover · Default: Hover"), as forced copies (below). A state the spec names but the visual source lacks shows the default with a hotpink label, "state missing" (`figma` mode) or "no visual change" (`spec-only`); its look is never invented.
3. **Variants.** Each variant beside the default, as forced copies, labeled with the property values that define it.
4. **Structure.** A table of what each part uses: property, token (or "none"), and resolved value. Literal values are marked hotpink. Derived behavior (for example a hover implemented as a scale) is noted here.
5. **AI can use this** (only with a Generative use block) and **Generated content** (only for a host): see "Generative UI" below.
6. **Spec content** (section 8).

**Forced copies** (states and variants) are generated from the component's template by the copy generator (section 10): `inert`, every `id` suffixed (`-state-hover`, `-variant-large`) with matching `for` and `aria-*` references, and the `.is-*` class added (`.is-hover`, `.is-active`, `.is-focus`, `.is-disabled` and similar). Only the demo is live.

**Focus** rings show on `:focus-visible`, drawn with `outline` (never `border`, which shifts layout), or with `box-shadow` plus a transparent `outline` so they still show in forced-colors mode. The forced `.is-focus` copy looks the same.

**Bundles** (ui, feature or scene): the bundle's page shows the assembled component; each part (`ui.modal.header`, `feature.navigation-bar.button`) is a sub-section with its own states and structure.

**Feature components** also show which ui component they build on, linked to its page.

**Library-backed components** (when `components.source` in `intent.md` isn't `custom`): the preview never runs the library's code. It rebuilds the component as for any other: in `figma` mode from the kit component and local overrides its `figma-nodes` point to; in `spec-only` mode as `spec-only-rendering.md`, "Library-backed components" says. The page header shows "Built on" with the docs link, and the demo is labelled "Library component, approximated". Only the options under "Used in this product" are demoed; deviations are shown as states or variants like any others.

**Generative UI:** the model is never called.
- **AI can use this** (a component with a Generative use block): a part on its page showing "Available as", the data contract as a table, the variants it may pick, and each of its worked examples rendered by stamping its template (section 10) with the example's sample data, labelled "Example output, not generated".
- **Generated content** (a feature or screen host): a part on the host's page showing:
  1. **Worked examples:** each example, with its request shown above it, rendered as its expected composition by stamping the allowed components' templates (section 10) with the example's sample data, inside the host's slot or region. Each is labelled "Example output, not generated".
  2. **Generation states:** the states around generation in the host's States table (for example generating, streaming, complete, partial, invalid, refusal), in its state switcher, each moving as its Motion cell names.
  3. **Fallbacks:** each row of the Fallbacks table, rendered as what it renders.
  4. **Allowed:** each allowed component, linked to its page, with where it goes; the composition rules as spec content.
- An example that uses a component that isn't AI-ready, isn't allowed in that host, or has no spec, or whose sample data breaks a data contract, renders as a hotpink placeholder and is reported ("Unresolved example").

**Scene components:** the Figma chrome (or `## Visual` chrome) rebuilt live, with the poster image from the spec's `assets:` pointer in the scene's place, and the specified states (loading, fallback, reduced motion, error) switchable. The scene content itself isn't rendered: label the poster "Scene content not rendered" (chrome, outside the scene). This isn't a gap; the overview key explains it. A poster that can't be reached is a missing-asset placeholder and an "Asset unavailable" gap.

---

## 6. Screens

- **One responsive build per screen.** In `figma` mode it is verified against each breakpoint frame the spec points to, and state frames are applied as overrides (`figma-mode.md`, section 2). In `spec-only` mode it follows the breakpoints `layouts.md` defines (its `## Visual` section), each rendered at its minimum width, using 375px for a breakpoint that starts at 0; say which widths were used. A breakpoint switcher, and a "fit to width" scale so a 1440px frame fits the content area without horizontal scroll.
- **Responsive rules** inside screens, components and layout diagrams are `@container` queries on the sized frame (`container-type: inline-size`), never viewport `@media` queries, so the breakpoint switcher and width control actually reflow the content. Viewport queries are for the chrome only.
- A `spec-only` screen without a `## Visual` section renders its components (styled by their own specs) in the region order the spec lists, inside the layout's page shell, with no extra spacing, and a hotpink "no Visual section" label.
- Live: components on the screen are stamped from their templates (section 10), with instance content, and keep their interaction.
- **States:** a switcher for the screen states the spec names (empty, loading, error, permission variants), using the spec's placeholder data.
- **Generated content:** a screen host shows the Generated content part as in section 5.
- **Components used:** a list linking to each component page. Components the screen uses but no spec covers or cites are listed in hotpink and reported as "Unspecced component".

---

## 7. Flows

- **Walkthrough:** the flow's steps in order. Moving between steps calls the motion helper with Route change, or Route change — top level between top-level destinations (or the default a step names in its System response). Each step shows the screen it happens on (the screen's build, scaled to fit), the action, and the outcome, with Next and Back controls and a step list to jump between steps; the current step has `aria-current="step"`. System-only steps (no screen in the steps table) show the action and system response only; they aren't gaps.
- **Branches:** each branch from the spec's branches table, selectable, showing where it leaves the main path and where it rejoins or ends.
- **Postconditions** listed after the last step.
- Steps on screens with no spec show a labeled placeholder and are reported as "Missing spec".
- A prototype link in the flow header, if any, is shown as a link only.

---

## 8. Spec content

At the end of every page, the spec itself, so the designer reviews visuals and decisions together. Foundation pages split from one spec (Color, Typography… from `tokens`) each show their own section; the spec's front matter and its other sections (such as Naming) appear on the first of those pages.

- **Acceptance criteria** first, as a list with their IDs exactly as written in the spec (never auto-numbered; skipped numbers stay skipped). The full ID (`spec-name/AC-n`) is visible, not shown only on hover, and has a copy button.
- Then the spec's remaining sections, formatted (tables, lists, code with a copy button), in the spec's order. In `spec-only` mode the `## Visual` section is included.
- Front matter as a small metadata block.
- References to other specs (`ui.button`, `screen.request`) become links to their pages, or are marked hotpink if the spec doesn't exist or is `deprecated` (a broken reference).
- **Rendering:** spec content is embedded in the page as escaped text and rendered by the markdown renderer (section 10). It never runs as markup or script.

---

## 9. Chrome and style rules

- **Chrome is neutral:** sidebar, headers, labels and tables are styled only with the preview's own `--dsp-*` custom properties (fixed neutral values), never the project's tokens, so the design system is the only thing styled by its tokens.
- **Chrome accessibility:** chrome meets WCAG 2.2 AA:
  - Text contrast 4.5:1; control borders, focus indicators and marks 3:1 against their background (1.4.11). Chrome text is at least 14px.
  - Visible `:focus-visible` focus, keyboard-operable navigation and controls, targets at least 24×24px.
  - Control roles: the motion-level control and theme switcher as in section 1; state, breakpoint and data switchers as a radio group or `aria-pressed` buttons; the flow step list with `aria-current="step"`; copy buttons with accessible names ("Copy `ui.button/AC-2`").
  - One polite live region (`aria-live="polite"`) announces results that don't move focus, such as "Copied" or a switched state.
  - `scroll-padding-top` equal to the top bar's height, so focused and linked targets aren't hidden under it.
  - Reflow at 320px without horizontal page scroll: wide tables and frames sit in scrollable wrappers (focusable, with a label).
  - Chrome has no animation, or honors `prefers-reduced-motion`.
- **Hotpink** (`#ff69b4`) means "missing or guessed": a gap (`gap-report.md`). It always comes with a text label, so it never relies on color alone:
  - **Fill** (missing tokens and values): hotpink on the property that's missing (fill, border or text color), with near-black text (`#1b1f24`, which passes AA; white on hotpink doesn't) where text sits on it.
  - **Placeholder** (missing assets, states and nodes): a dashed double stroke, hotpink with `#1b1f24` beside it, so the outline holds 3:1 on light and dark surfaces.
  - **Guessed** (ambiguous `spec-only` visuals, guessed motion): the guess rendered as is, never recoloured, with the same dashed double stroke and a label outside it.
  - **Label:** a hotpink text label beside the item.
  Nothing else in the chrome uses pink.
- **Neutral mark** (setup limits: Ready for dev unknown, font fallback, assets blocked by the environment): a grey dashed outline or "unknown" label in the chrome's neutral colors, never hotpink.
- **Labels** for forced states and placeholders are chrome, outside the component, so they never affect its layout.
- **Self-contained:** see `build-process.md`, step 4.

---

## 10. Inline helpers

Three small inline scripts, each built once and reused everywhere. None makes a request.

### Motion helper

Built in its own phase right after the shell (`build-process.md`, step 5), only if the motion pages exist. Each row of `motion.defaults` is built once, keyed to `data-motion`, and called by name, for example `dsp.play(el, '<default name>')`. Components, screens, flows, worked examples and the theme switch call it; they never re-implement choreography. The Motion foundations page is its showcase (section 4).

- **Timing:** durations, easings and distances come from the `motion.tokens` custom properties, never literal values.
- **Motion levels:** under `data-motion="reduced"`, each default plays its `motion.reduced` row, or the spec's own reduced-motion behavior when the caller passes one. Under `data-motion="none"`, the state is set with no animation at all, and loops (skeleton pulses, indeterminate indicators) show "Loading…" text instead.
- **Moments:** `(moment)` is stripped before the name is looked up. Baseline has one form for both tiers, so the mark changes nothing unless a customized `motion.defaults` gives moments their own form.
- **Springs, scale and blur:** a `spring-*` easing always runs at its paired `spring-*` duration. Easing values are used as CSS easing functions as written, including `linear()`. Scale and blur come from their tokens; blur ends at 0 with the filter removed.
- **Forms:** `<a> + <b>` runs both defaults together on the same change; `<enter>; exit: <exit>` plays the exit default when the element leaves.
- **MOT-5:** a new call on an element that is still animating first jumps the running animation to its finished state, then starts.
- **MOT-11:** an entering element is hidden from before its entrance starts, through any delay or stagger.
- **MOT-13:** inside a clipping container, content that would travel, scale or overshoot fades instead; text is never clipped.
- **MOT-14:** each step starts *then* (when the previous one finishes) or *with* (at the same moment), as the default says.
- **MOT-12 by construction:** the resting look is always the base CSS of the state the element is in. The state's class or attribute is set first; the animation runs from the previous look to it. Animations never use `forwards` fill and never leave inline styles behind, so when they end or are interrupted, the element looks exactly as it does with motion off.
- Focus is never animated (MOT-3).

### Copy generator

Each component's build is written once, inside a `<template>` named after its spec. Inline JS stamps the live demo from it and generates the forced copies: it clones the template, suffixes every `id`, rewrites `for` and `aria-*` references to match, sets `inert`, and adds the `.is-*` class. Screens, flows and worked examples reuse component builds the same way, applying instance content (text, variant, slot content) after cloning; those copies stay live. Each component's behavior is one init function, run on every live copy.

### Markdown renderer

A tiny inline renderer, under about 60 lines. It escapes all text first, then formats. It handles only headings, paragraphs, tables, lists, inline and fenced code, bold and italic, and links, and renders inline code at the end of a table cell and code inside bold correctly. Spec content is embedded in the page as escaped text and rendered by it at load. Front matter is split off before rendering and shown as the metadata block (section 8).
