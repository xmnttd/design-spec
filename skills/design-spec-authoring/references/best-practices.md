# Best Practices

Shared rules for the design-spec family: what makes a spec good, in either mode. Ownership is in `source-of-truth.md`, types and structure in `spec-types.md`, and `spec-only` visual rules in `spec-only-mode.md`. This file doesn't repeat them. Figma file recommendations are in `figma-hygiene.md` (`figma` mode), and guidance for generative UI and AI chat in `genui.md`.

Apply these continuously while writing, not only at save.

---

## Contents

1. Specs are decisions
2. Requirement language and precision
3. Front matter and titles
4. Token principles
5. States and variants
6. Behavior completeness
7. Accessibility
8. Acceptance criteria quality
9. Keep specs lean
10. Stub cleanup

---

## 1. Specs are decisions

A spec says what is decided, current and consistent. It is what engineering builds from.

- No open questions, TBDs, "confirm before build" notes, or gap flags.
- No history ("previously this was…") and no argument ("we chose X because Y was rejected…"). Rationale for a decision goes in `report/decision-log.md`; findings go in the audit register. Two history lines are allowed: the `Retired:` line of removed criterion IDs (`spec-types.md`, §9), and a deprecated spec's `Replaced by` line, directly under its Role line (`lifecycle.md`, "References").
- If something is unresolved and blocks the entry, it is raised with the designer; if it doesn't block, the entry is written with what is decided and the gap is left to the audit (`work-path.md`, "Two rules that apply at every step").
- When a change invalidates reasoning elsewhere on the page, that reasoning is updated too, so the page never contradicts itself.

---

## 2. Requirement language and precision

- Behavior, logic and accessibility rules use BCP 14 terms (RFC 2119 and RFC 8174): **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, **MAY**. Only the uppercase forms are normative; lowercase "must" or "may" is ordinary English, so a requirement is always written in uppercase. Use them deliberately; "MUST" is a build requirement, "SHOULD" allows a justified exception, "MAY" is optional.
- Be definitive. "Approximately", "slightly", "standard", "typical", "as needed" and "left or right" are not specs.
- Use one name per thing across the whole spec set. If a component is "panel" in one spec, it isn't "card" in another.
- **Placeholder data only.** Examples, sample content and data in specs use invented placeholders. Never include real customer, user or personal data, credentials or confidential figures.

---

## 3. Front matter and titles

- Front matter MUST be valid YAML. Quote any text value that contains a colon followed by a space or ` #`, or that starts with `[`, `{`, `*`, `&`, `!`, `|`, `>`, `%` or `@`: `endpoint: "documentation platform: Confluence, Design space"`. Also quote node IDs, versions and any other text that could parse as a number, date or boolean: `nodes: ["123:456"]`, `figma-version: "1234567890"`, `version: "3.2"`. `last-updated` stays an unquoted `YYYY-MM-DD` date. Lists of names such as `parts: [ui.modal.header, ui.modal.body]` stay unquoted once filled (stubs quote their `[…]` placeholders only so the template parses). An invalid block breaks previews and machine reading.
- Fields follow the fixed order in `spec-types.md`; fields that don't apply are omitted, never left empty.
- **Titles:** `title` is the spec name (filename minus `.md`): `feature.panel-order-summary`, `tokens`, `intent`. On a documentation platform it is also the page title. Specs have no written H1: platforms render the title as the page heading, and locally the filename does the same job. The Role line (or the summary in `intent.md`) carries the human-readable description. The project name is recorded once, in `intent.md` (`project`).

---

## 4. Token principles

These apply in both modes. In `spec-only` mode they are the authoring rules for `tokens.md`. In `figma` mode Figma's variables stay canonical, and these are assessment criteria, not impositions.

### Completeness

Five categories: **color · spacing · typography · elevation · shape and size** (radius, border widths, component sizing such as control heights and icon sizes). This list is the one owner of the visual categories; other files point here. `icons.md` references icon-size tokens by name and never holds values. Motion tokens (durations, easings, distances, delays) live in `motion.tokens`, not here. Layout values live in `layouts.md`.

### Tiers

| Tier | Structure | Best for |
|---|---|---|
| 1-tier | Tokens applied directly to components | Small, single-brand projects |
| 2-tier (recommended default) | Primitives define the palette; semantics map purpose to primitives; specs reference semantics only | Most projects |
| 3-tier | Adds a component token layer | Multiple brands, or themes beyond light and dark; higher maintenance |

The chosen tier is recorded in `tokens.md`, and the file is structured to match it.

### Naming

- Primitives by value: `--color-gray-900`, `--space-16`.
- Semantics by purpose: `--color-text-primary`, `--color-status-error`.
- Pattern: `--[category]-[property|role]-[variant]-[state]`, omitting segments that don't apply (`--color-text-primary`, `--color-action-primary-hover`).
- **Focus ring:** `--color-focus-ring` follows the pattern. `--focus-ring-width` and `--focus-ring-offset` (shape and size) are a named exception, kept short because every interactive spec uses them (`spec-only-mode.md`, §4).
- Ask whether the project already has a convention before proposing one. When extracting from a prototype, normalize to the convention; don't preserve ad-hoc names.
- In `figma` mode the Figma variable path is canonical, and `tokens.md` documents the transform to CSS custom properties (e.g. `color/text/primary` → `--color-text-primary`).
- **Library-backed sets:** token names follow the library (MD3 system tokens such as `md.sys.color.primary`, exposed on the web as `--md-sys-color-primary`; shadcn CSS variables such as `--primary` and `--primary-foreground`). Tiers map to the library's structure (MD3 separates reference, system and component tokens). The pattern above applies only to tokens the product adds. Which name is canonical when the library, the Figma path and the set's existing names differ: `source-of-truth.md`, "Token names in library-backed sets". Motion token names always stay `--motion-*`; a motion value may name the library token it came from.

### What to check

- **Missing semantic layer** (2- and 3-tier only): primitives only, which forces specs to hard-code intent.
- **Scale gaps:** spacing or type scales with unexplained jumps.
- **Font sourcing and licensing:** recorded for every family.
- **In `figma` mode also:** fills and text styles not bound to variables, inconsistent variable naming, and modes missing values.

In `figma` mode, raise only issues that block good specs, briefly, and offer fixes in Figma only with the designer's approval. Ongoing detection is the audit's job, using these same principles.

---

## 5. States and variants

Every interactive component defines, at minimum: **default, hover, active (pressed), focus, disabled.**

Add these wherever they apply:

- **Empty:** any component that displays dynamic data.
- **Loading:** any component that fetches or waits.
- **Error:** any input, form or data component, including the error message and how it is announced.
- **Selected:** tabs, toggles, checkboxes, options in a list.
- **Current:** the navigation item for the current location (`aria-current`).

"Active" means pressed only; never use it for selected or current.

Rules:

- State and variant names match Figma's exactly (`figma` mode).
- If a needed state is missing in Figma, the spec still names it and describes its behavior. It does not invent the visuals; the missing variant is a finding.
- Defaults that every spec inherits (empty, loading and error handling) live in `intent.md`; motion defaults and reduced-motion behavior live in `motion/`. A spec states only where it differs.
- **Motion column:** ui, feature and scene components and screens carry a Motion column in their States table. Every row holds one of the forms the project's `motion` page allows: never blank, TBD or a value. When nothing fits, use `Default: Fallback`. Mark a rare change that matters with `(moment)` (`Default: Content entrance (moment)`): baseline ignores it, and a style applied later gives it more character. The forms and rules are owned by `motion` ("How specs reference motion", "Exceptions"); read it before filling the column. A spec's own reduced-motion behavior goes in its Accessibility section (§7); other exceptions go in Behavior.
- **Variants describe deltas only.** Never re-spec properties that don't change.
- **Library-backed components:** the library's built-in states count as present. The States table lists only states the product adds or changes; where a state uses the library's own animation, its Motion cell uses the `Library:` form (`motion`). If you can't confirm what the library does for a state the product needs, keep the state and its behavior in the spec.

Conversational states for hosts of an AI assistant, and generative UI guidance: `genui.md`.

---

## 6. Behavior completeness

Watch for these gaps while writing:

- **Content constraints:** maximum lengths, truncation and overflow behavior, pluralization.
- **Input constraints:** input type, allowed formats, maximum length, validation rules and when validation runs.
- **Sort order:** for any list, menu or ordered data set, the order and whether it is configurable.
- **Dismissal and focus return:** for anything that opens (menus, dialogs, popovers): how it closes and where focus goes.
- **Data edge cases:** empty, very long, missing optional fields, very large counts.

---

## 7. Accessibility

Default target: **WCAG 2.2 AA**, unless `intent.md` says otherwise.

- **Behavior in the spec:** roles, accessible names, keyboard interaction, focus order and announcements for dynamic changes.
- **Composite widgets** (menu, dialog, tabs, combobox, listbox and similar) link the specific WAI-ARIA Authoring Practices pattern page (for example `https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/`) instead of re-specifying keyboard behavior. Document deviations only.
- **Focus** is always visible. Focus indication MUST NOT shift layout (in CSS terms, `outline`, not `border`; see `spec-only-mode.md`, §4).
- **Focus not obscured** (2.4.11): a focused element is never fully hidden by sticky headers, footers or other overlaid content.
- **Target size:** at least 24×24px (WCAG 2.2 AA); 44×44px recommended for touch. The project's default lives in `intent.md`; a spec states only where it differs.
- **Icon-only controls** have an accessible name; decorative icons are hidden from assistive technology.
- **Disabled states** are communicated by more than color alone.
- **Motion** is on by default. Reduced-motion behavior follows `motion.reduced`. A spec's own reduced-motion behavior, where it differs, goes in its Accessibility section. A state that uses a default listed under "Announcements" in `motion` says in Accessibility what is announced (placeholder text); the live region follows `motion`. Optical effects are never the signal (MOT-4): a spec never names an optics token and opts out only with `Optics: none` in Behavior. Moving, blinking or auto-updating content that starts on its own and lasts more than five seconds MUST have a way to pause, stop or hide it (2.2.2).
- **Reflow** (1.4.10): content MUST work at a width of 320 CSS px without scrolling in two directions, except content that needs two dimensions (maps, data tables).
- **Content on hover or focus** (1.4.13): tooltips and similar popups MUST be dismissable without moving the pointer or focus, stay open while the pointer is over them, and stay until dismissed or no longer relevant.
- **Text resize and spacing** (1.4.4, 1.4.12): text MUST resize to 200% and take increased line, paragraph, letter and word spacing without clipping or overlap.
- Reflow, content on hover or focus, and text resize and spacing are behavior: write them as criteria where a component's layout or popups could break them.
- **Dragging** always has a non-drag alternative.
- **Redundant entry** (3.3.7): information the user already gave in the same process is filled in or offered for selection, not asked for again.
- **Accessible authentication** (3.3.8): sign-in never depends on a cognitive test (remembering, transcribing or solving) without an alternative; allow paste and password managers.
- **Consistent help** (3.2.6): help mechanisms that appear on several screens stay in the same relative order.
- Visual checks (text contrast, non-text contrast for controls and focus indicators (1.4.11), focus appearance) depend on Figma values in `figma` mode and on token values in `spec-only` mode. Contrast checks and deeper audits are recommended to a dedicated accessibility review.

---

## 8. Acceptance criteria quality

Placement (which level owns which criterion) is in `spec-types.md`. Each criterion also has to be checkable:

- Given / When / Then, using MUST. A **Then** MAY state several outcomes joined by "and", as long as each can be observed and checked on its own.
- Specific: "focus MUST move to the first invalid field", not "the form handles errors well".
- Cover the happy path plus edge, error and empty cases.
- Don't restate the motion rules (MOT-1 to MOT-14 in `motion`) as criteria; they're checked once, at the motion level. Write a criterion only for motion behavior the spec owns, such as an exception.
- Number criteria so QA, engineering and the audit can cite them. ID rules: `spec-types.md`, §9, "Criterion IDs".

---

## 9. Keep specs lean

- **Reference, don't restate.** A screen cites its components; a flow cites its screens; a feature cites the ui component it builds on. None of them repeats another spec's content. A library-backed spec links to the library's docs instead of restating them (`source-of-truth.md`, "Component libraries").
- **Defaults live once,** in `intent.md` (motion defaults in `motion/`).
- **Include sections only when they apply.** An empty or boilerplate section is noise for agents and people.
- Prefer tables for anything enumerable (states, fields, events, steps).
- If a spec is growing past what one person can review in a sitting, consider whether it is two specs, or whether something belongs in a referenced spec.

---

## 10. Stub cleanup

Before a spec is saved:

- Remove every stub comment (the main block and any short `<!-- … -->` notes) and every tag (every tag defined in the stub's comment, for example `[figma]`, `[spec-only]`, `[bundle]`, `[library]`, `[genui]`, `[assistant]`, `[platform]`, `[if applicable]`). In front matter, remove the whole `# […]` comment that carries a tag.
- Remove blocks for the other mode, bundle blocks for non-bundles, and sections that don't apply.
- A placeholder written `[mode: text]` (for example `[spec-only: value]`) applies only in that mode: fill it in that mode, and remove it in the other.
- Remove every unfilled `[placeholder]`, and replace `YYYY-MM-DD` with today's date.
- Confirm front matter is valid YAML and in the fixed order.

**Shells.** A shell is a spec with valid front matter only (`title`, `type`, `status: draft`, `last-updated`, and for `intent.md` its project settings), no Figma fields until they are verified, and no body. Scaffolding (`work-path.md`, step 2) and missing referenced specs create shells. A shell is exempt from these cleanup rules and from "Include sections only when they apply" (§9) until it is first filled; from then on they apply.
