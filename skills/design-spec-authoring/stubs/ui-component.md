---
title: "ui.[name]"
type: ui-component
status: draft
last-updated: YYYY-MM-DD
parts: ["ui.[name].[part-a]", "ui.[name].[part-b]"]   # [bundle]
figma-nodes:                      # [figma]
  file: "[Figma file URL]"
  nodes: ["[main component or component set node ID]"]
  library-key: "[published key of the kit component]"   # [library]
figma-version: "[version or lastModified]"   # [figma]
assets:                           # [if applicable]
  - name: "[asset name]"
    format: "[e.g. svg, png]"
    location: "[repository path or URL]"
    version: "[version]"
---

<!--
STUB — UI COMPONENT   (filename: ui.[name].md · bundle: ui.[name].bundle.md)
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [bundle] keep only for bundles
       [genui] keep only if the model may use this component (or one of its parts) in generated content
       [library] keep only for a library-backed component (intent `components.source`
                 is third-party, internal or mixed, and this one comes from the library)
       [if applicable] keep only when relevant

A generic, non-prescriptive building block: placeholder content, no fixed
data shape, reusable across unrelated screens.
figma mode: no visual values here; Figma owns how it looks.
State and variant names match Figma exactly.
No acceptance criteria: ui components verify against states and variants.
Front matter: `assets` lists files this component depends on (a logo,
an illustration), as pointers only. In a library-backed spec, `library-key`
points at the kit component and `nodes` holds only a local override
component; with no override, drop `nodes`.
Library-backed: the library's docs (at the version in intent) own anatomy,
built-in states, keyboard and ARIA behavior. Don't restate them. Keep Role,
the Library block, Usage, Properties (only the properties this product uses),
States (only what the product adds or changes), Variants used, Behavior and
Accessibility only where they deviate. Remove Anatomy unless it deviates.
If you can't confirm what the library does for a state or behavior the product
needs, keep it in the spec as a requirement; never drop it on an assumption.
A library-backed bundle keeps its Parts when other specs cite them.
-->

**Role:** [One sentence: the job this component does in the UI.]

---

## Parts

[bundle]

<!-- Parts are named with the bundle's base name plus the part, matching Figma (ui/[name]/[part]). Each part adds only what differs from the parent. Parts are never used on their own; if one is, promote it to its own spec. -->

- **`ui.[name].[part-a]`:** [what it contains and does.]
- **`ui.[name].[part-b]`:** [ ]

---

## Library

[library]

<!-- "Used in this product" owns the allowed options. "Property map" only maps this spec's property names to library props. The Properties section lists only the properties this product uses, never a second list of options. If the component uses different library components by context, list each under Built on with when (for example Drawer at compact width, Dialog at regular width). For a bundle, name the library sub-component each part maps to. -->

- **Built on:** [library component and variant, e.g. MD3 "Filled button", shadcn `Button` `variant="default"`], [link to its docs at the version in `intent.md`].
- **Property map:** [each of this spec's properties and the library prop it maps to, for example `intent` → `variant` (`secondary` → `"outline"`). Keep the product's property names that other specs cite; never rename them just to match the library.]
- **Used in this product:** [the variants, sizes and options allowed here. Anything the library offers that isn't listed is out.]
- **Deviations:** [every override or change from the library, each with its behavior, states and accessibility; or "None".]
- [if applicable] **Copied code (shadcn and similar):** every edit to the copied component is a deviation and is listed above. Animation classes in it are raw motion values: replace them with motion tokens (a deviation) or keep them as `Library:` motion.

---

## Usage

- **Use when:** [situations where this is the right component.]
- **Don't use when:** [situations where it's wrong.]
- **Alternatives:** [`ui.[other-component]` for …]

---

## Anatomy

<!-- In visual order: block-start to block-end, then inline-start to inline-end. -->

1. **[Element]:** [what it is; required or optional; content constraints such as max length or truncation.]
2. **[Element]:** [ ]

---

## Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| [name] | [string, boolean, enum, slot, handler] | [default] | [ ] |

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | — | [Reference state] | — |
| hover | [Pointer over] | [What changes in behavior, if anything] | Default: Hover |
| active | [Pressed] | [ ] | Default: Press |
| focus | [Keyboard focus] | [Focus is visible] | Default: Focus |
| disabled | [Condition] | [Not interactive; why it's disabled is communicated by …] | Default: Disabled |
| [if applicable] [loading / error / empty / selected] | [ ] | [ ] | [Default: name / None — reason] |

<!-- Motion: names from `motion.defaults` only, never values (`motion`, "How specs reference motion"). The default row keeps `—` unless the component appears or disappears, loops, or shows a changing value; then it names that motion (a skeleton: Default: Loading). A variant switched at runtime (checked, selected, expanded) names its motion in its "Differs by" cell. Add `(moment)` when the change is rare and matters (`Default: Surface enter — modal (moment)`); baseline ignores the mark, a style applied later uses it. -->

---

## Variants

<!-- Deltas only; never restate the default. -->

| Variant | Differs by |
|---|---|
| [e.g. size=sm] | [ ] |

---

## Behavior

- [Interaction rule, using MUST / SHOULD / MAY.]
- [Input constraints, sort order, overflow and truncation, if applicable.]
- [if applicable] `Optics: none` where this component must never carry an optical effect (a dense table row, a destructive action).

---

## Generative use

[genui]

<!-- Marks this component AI-ready: without this block the model never uses it. One block for the component. [bundle] One block per AI-ready part instead, headed by the part name. Don't name hosts here: each host's Generated content lists what it allows. Nesting: at most two levels, no cycles, and every slot points to an existing spec (`genui.md`, §4). -->

[bundle] ### `ui.[name].[part-a]`

- **Available as:** [`standalone` (a host may place it directly) | `nested` (only inside another AI-ready component) | `both`].
- **When to use:** [one or two sentences written for the model: the question or situation this component answers.]
- **Data contract:** [every field the model fills: name, type, limits, required or optional, and which must come from real data, never invented. For a slot filled by another AI-ready component, reference its contract instead of restating it, e.g. "chart: per `ui.[other]`, variant `[variant]`".]
- **Variants it may pick:** [only the variants the model may choose, e.g. `embedded` inside a card.]
- **When activated:** [what happens when the person interacts with it inside generated content: for example, opens a screen or starts a flow. Actions never run from generated content unless this spec says so.]
- **Never:** [anything the model must not do with it.]
- **Worked examples:** [1–3: sample data → what renders.]

---

## Accessibility

- **Role:** [native element or ARIA role.]
- **Accessible name:** [source.]
- **Keyboard:** [Follows the WAI-ARIA Authoring Practices "[pattern]" pattern ([pattern page URL]). Deviations: [none | list].]
- **Focus:** [order and visible focus.]
- [if applicable] **Target size:** [only where it differs from the default in `intent.md`.]
- [if applicable] **Announcements:** [for states that use a default listed under "Announcements" in `motion` (Confirmation, Validation error, Pop-in, a toast, Loading, an In-place content change the person didn't cause, a keyboard Reposition): what is announced, with placeholder text. The live region follows `motion` unless stated.]
- [if applicable] **Reduced motion:** [only where this component differs from `motion.reduced`.]

---

## Visual

[spec-only]

<!-- Token references only; no literal values. A library-backed spec with no overrides has no Visual section; with overrides, list only those (`spec-only-mode.md`, §1). -->

- **Surface:** [background, border, radius, elevation tokens]
- **Typography:** [font token and color token per text element]
- **Spacing:** [padding and gap tokens]
- **Icons:** [icon names from icons.md, size token, color rule]
- **Focus:** `outline` with `--focus-ring-width`, `--focus-ring-offset` and `--color-focus-ring`, never `border` (`spec-only-mode.md`, §4)
- **States:** [visual delta per state]
- **Variants:** [visual delta per variant]
