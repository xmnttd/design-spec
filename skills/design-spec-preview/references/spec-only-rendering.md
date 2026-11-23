# Spec-Only Rendering

How preview renders a spec set that has no Figma (`visual-source: spec-only`), and specs not yet migrated during a move to Figma. How these visuals are written is in `../design-spec-authoring/references/spec-only-mode.md`; read it first.

In this mode the preview is the designer's **only** way to see the design, so render everything the specs describe, and mark everything you had to guess.

---

## Sources

| What | From |
|---|---|
| Token values, modes | `tokens.md` only |
| Component and screen visuals | Each spec's `## Visual` section (token references); library-backed components as below |
| Layout values (breakpoint widths, columns, max width; gutters and margins as spacing token references) | `layouts.md` `## Visual` section |
| Icons | `icons.md`: the named installable library or asset file pointers |
| Fonts, logos, imagery | Asset pointers in `tokens.md` and specs |
| Scene posters | The scene spec's `assets:` pointer |
| Motion | `design-system/motion/` (`motion.tokens` values, `motion.defaults`, `motion.reduced`) and each spec's Motion column, played through the motion helper as in `figma` mode (`page-anatomy.md`, sections 5 and 10) |
| Behavior, states, accessibility, criteria | The spec's other sections |

---

## Tokens

- Emit every token in `tokens.md` as a CSS custom property in `:root`, using the names as written. Semantics reference their primitives (`--color-text-primary: var(--color-gray-900)`), never a repeated value.
- Theme or brand modes become `[data-theme="…"]` or `[data-brand="…"]` blocks overriding semantic tokens.
- Composite type tokens follow the text-style rule in `page-anatomy.md`, section 4.
- A token referenced in a spec but missing from `tokens.md`: render hotpink and report it. If `tokens.md` itself is missing, that's one gap (`gap-report.md`, "Missing token").
- A literal visual styling value in a spec (a hex code, a pixel size) instead of a token: render it so the design is visible, and report it as "Literal value" (motion values and behavioral constants aren't literal values).

---

## Reading `## Visual` sections

`## Visual` sections are written in CSS terms (`spec-only-mode.md`, section 3), so translate them directly:

- `fit-content`, `100%`, `flex: 1`, `overflow: hidden`, `outline` and offsets mean what they say in CSS.
- State and variant lines are deltas from the default: apply only what they change.
- Positions of floating elements come from the logical `inset-*` token references (`inset-block-start`, `inset-inline-end`, …; `../design-spec-authoring/references/spec-only-mode.md`, section 5).
- **Anything relative or vague** ("slightly larger", "near the edge"): make the most reasonable choice with existing tokens and render it as is, never recoloured, with the guessed-value marking (a dashed double-stroke outline and a label outside the element, `page-anatomy.md`, section 9). Report it as "Ambiguous visual". Never leave it unrendered.
- A ui, feature or scene spec with no `## Visual` section: render the structure from its anatomy, unstyled, with a hotpink label, and report it. A library-backed ui spec with no overrides is exempt: it needs no `## Visual` section (below). Screens without one are in `page-anatomy.md`, section 6.
- `layouts.md` with no `## Visual` section: the Layouts page shows its rules without values, screens render in one 375px frame, and it's reported.

---

## Library-backed components

The library's default look with the project's theme is the visual (`../design-spec-authoring/references/spec-only-mode.md`, section 1). Render it only from the theme tokens in `tokens.md`, the spec's Library block and its `## Visual` overrides, never from memory of the library. Anything those don't define is rendered as a placeholder marked as guessed, and reported as "Ambiguous visual".

---

## Assets

- **Icons from a named library:** if the library's files can be added to `assets/` (an installed package in the project, or files the designer provides), use them. Otherwise, a labeled placeholder per icon, reported once per library.
- **Asset file pointers** (including a scene's poster in `assets:`): copy the file into `assets/` if the location is reachable; otherwise a labeled placeholder, reported as "Asset unavailable".
- **Fonts:** see `build-process.md`, step 4.
- Never draw an icon or illustration yourself, and never substitute a look-alike.

---

## Mid-migration

When `visual-source` is `figma` but some specs still have `## Visual` sections (see "Migration" in `../design-spec-authoring/references/source-of-truth.md`):

- Render each spec from the source it has: `figma-nodes` through `figma-mode.md`, `## Visual` through this file.
- Label each page's source in its header ("Visual source: Figma" or "Visual source: spec").
- **Figma variables win.** Where a token name exists both as a Figma variable and in `tokens.md`, use the Figma value for every spec. Use a `tokens.md` value only for a name Figma doesn't define, and report each one as "Unmigrated token".
- A spec with both `figma-nodes` and `## Visual` is duplicated ownership: render from Figma, and report it once. Values inside that `## Visual` section aren't also reported as literal values.
