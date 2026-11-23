# Spec-Only Mode

Shared rules for the design-spec family, for **`spec-only` mode only** (`visual-source: spec-only` in `intent.md`). In `spec-only` mode there is no Figma, so the spec set owns the visual layer. These rules keep that layer precise enough to build from, and isolated enough to move to Figma later without rewriting the rest.

In `figma` mode none of this applies: visual values live in Figma, and specs hold pointers (see `source-of-truth.md`). Token principles (tiers, naming, completeness) apply in both modes and live in `best-practices.md`.

---

## Contents

1. Where visual values live
2. Token values in `tokens.md`
3. CSS terms, not design-tool terms
4. Focus
5. Dimensions and positions
6. Precision
7. Icons and other assets
8. Visual check
9. Figma comps from the specs (optional)
10. Moving to Figma later

---

## 1. Where visual values live

This section owns the spec-only layout-values rule and which specs carry a `## Visual` section.


- **Visual styling values live only in `tokens.md`.** Color, type, spacing, radius, shadow and elevation values are defined there and nowhere else.
- **Every other spec references token names**, never literals. A value used once still becomes a token. If no suitable token exists, add one to `tokens.md` first, then reference it.
- **Each ui, feature, scene and screen spec has a `## Visual` section** holding all of its visual references. The exception is a library-backed ui spec with no overrides: the library's default look with the project's theme is its visual, so it has no `## Visual` section. With overrides, its `## Visual` section holds only those, as token references. Behavior, states logic, accessibility and acceptance criteria stay in their own sections. Keeping the visual layer in one section is what makes migration to Figma a clean lift-out.
- **`layouts.md` owns layout values** in its own `## Visual` section: breakpoint widths, column counts, max content width and z-index are written there as values; gutters and margins reference spacing tokens. These are the only visual values written outside `tokens.md`, and layout values never go in `tokens.md`. Motion values are separate: they live in `motion.tokens`, never in `tokens.md` (`source-of-truth.md`).
- **Flows have no `## Visual` section.**

What each `## Visual` section covers:

| Type | Covers |
|---|---|
| ui-component, feature-component | Surface, typography, spacing, borders, radius, elevation, iconography, and the visual delta for each state and variant |
| scene-component | The chrome around the scene (controls, hotspot labels, legends) and its placement, plus art direction for the scene content in words |
| screen | Composition and spacing between regions; everything else comes from the components and layout it cites |
| `layouts.md` | Breakpoint, grid, max content width and z-index values |

---

## 2. Token values in `tokens.md`

`tokens.md` is the only place visual styling values are written (layout values: §1). Apply the token principles in `best-practices.md` (tiers, naming, completeness), then:

- Write every value once, on the token that owns it. Semantics reference primitives by name, not by repeating the value.
- Components aren't written until the five token categories (`best-practices.md`, §4) are in place.
- **Theme and brand modes**, only if the project has them: list the mode names and, for each semantic token that varies, its value per mode. Primitives don't vary by mode.
- **Fonts:** record the font source. If the font isn't freely available from a public font service, record the license, the file format (woff2) and where the font files are stored, as an asset pointer.
- **DTCG export:** on request, authoring MAY generate a `tokens.json` in the W3C Design Tokens Format (2025.10) from `tokens.md`. It is generated output, not a spec (`spec-types.md`, §4): `tokens.md` stays the owner. Save `tokens.json` outside the spec tree (next to `design/`, or where the designer names), never kept as a maintained copy: regenerate it from `tokens.md` on request.

---

## 3. CSS terms, not design-tool terms

Specs are read by engineers and their agents, so visual descriptions use CSS, not design-tool vocabulary.

| Design-tool term | CSS |
|---|---|
| hug / hug contents | `fit-content` |
| fill / fill container | `flex: 1` on the main axis; `align-self: stretch` on the cross axis |
| clip | `overflow: hidden` |
| fixed (sizing mode) | explicit size via a token |
| stroke | `border` or `outline` |
| auto layout | `display: flex` with direction, gap and padding |
| corner radius | `border-radius` |
| layer opacity | `opacity` |

---

## 4. Focus

`border` is part of the box model and shifts layout, so focus MUST NOT use `border`. Use:

```css
outline: var(--focus-ring-width) solid var(--color-focus-ring);
outline-offset: var(--focus-ring-offset);
```

or `box-shadow` with the same tokens when more control is needed. With `box-shadow`, also set `outline: var(--focus-ring-width) solid transparent`, so focus stays visible in forced-colors mode, where shadows are removed.

---

## 5. Dimensions and positions

Relative language ("near the edge", "bottom corner") is not a spec.

- State explicit dimensions where a size is constrained, as token references; otherwise `fit-content` or `100%`.
- Any floating element (badge, tooltip, dropdown, popover) states its offsets with logical properties (`inset-block-start`, `inset-inline-end`, `inset-block-end`, `inset-inline-start`) as token references, so they follow right-to-left layouts.
- State the visual order of elements in logical terms: block-start to block-end, then inline-start to inline-end.

---

## 6. Precision

- Be definitive. "Approximately", "slightly", "standard" and "typical" are not values.
- **Decide; don't defer.** In `spec-only` mode the spec set owns visuals, so a missing visual decision is made using existing tokens and established patterns, documented as final, and confirmed at the checkpoint. It is not left as an open question in the spec.
- Variants describe only what differs from the default. Never re-spec unchanged properties.

---

## 7. Icons and other assets

- Icon artwork comes from a **named, installable library** (record name, license and install method in `icons.md`) or from **asset file pointers** (location, format, version). Never inline SVG.
- Logos, illustrations, imagery and font files follow the same pointer rule.
- Every icon referenced in a spec exists in `icons.md`.

---

## 8. Visual check

In `spec-only` mode the preview is the designer's only way to see the design. Before visuals are treated as approved, recommend running `design-spec-preview` so the designer can review them rendered.

---

## 9. Figma comps from the specs (optional)

A spec-only set MAY have Figma comps generated from it (screens and components drawn in Figma for stakeholder reviews, presentations or prototyping). They are derived output, like the preview, never a source.

- **Only in spec-only mode.** In `figma` mode Figma is already the source; don't create a second file of comps.
- **Recorded once,** in `intent.md` as `figma-comps` (the file URL). Never in a spec's `figma-nodes`, which would make Figma a source.
- **Generated on request,** with the designer's approval (a write to Figma, `connections.md`, "Writing to Figma"), following Figma's own skills for building designs. Build from `tokens.md` values and each spec's `## Visual` section; for library-backed specs, use the library's Figma kit with the project's theme applied.
- **Labelled:** each comp's page or frame says "Generated from specs, YYYY-MM-DD; not a source" and names the specs it shows.
- **One way.** A change made in the comps comes back through authoring as a spec edit; the comps are then regenerated. Comps are never edited to differ from the specs.
- **Coding agents ignore them** (the `AGENTS.md` line, or intent's "For agents" section).
- **Becoming the source:** if the team decides the comps should be the design, that's a move to `figma` mode (section 10). The comps file can be the starting point; until then they stay illustrations.

---

## 10. Moving to Figma later

Everything above is arranged so that migration is mechanical: tokens move to Figma variables, each `## Visual` section is replaced by `figma-nodes` and `figma-version`, and `tokens.md` keeps names, tier and semantics. Components are named in Figma per "Matching names in Figma" in `spec-types.md`. See "Migration from `spec-only` to `figma`" in `source-of-truth.md`.
