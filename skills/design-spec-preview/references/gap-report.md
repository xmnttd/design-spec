# Gap Report

What preview records as a gap, and how it reports gaps in chat. Gaps never appear as a page in the HTML. The hotpink marks in the preview are their visual counterpart: each gap type below says where its mark goes, or "chat only" when it has none.

---

## Collecting gaps

Collect silently from the first read. **Don't stop the build for a gap:** make the most reasonable rendering choice, mark it hotpink, log it, and continue. Stop only for a blocker the designer must decide on (Figma unreachable), and raise it in the step 2 message. A missing `intent.md` stops the run earlier (`build-process.md`, step 1).

**Every "Couldn't read" is marked in the preview.** Anything rendered without its source (from the spec, a fallback or a placeholder) carries a hotpink mark and label, so the gap tables and the preview always match.

**"Couldn't check" is never "checked"** (`../design-spec-authoring/references/connections.md`, "When a connection is unavailable"). Anything the preview can't read is shown as a marked placeholder or "unknown" and logged, never filled from memory, guessed from a name, or reported as fine.

Log each gap once, at the place it should be fixed, with what was expected, what was found, and what the preview did. One gap per problem per spec location: several problems in one spec are several gaps; one problem that shows in several modes (a contrast pair failing in Light and Dark) is one gap listing the modes. When no type fits exactly, use the nearest and say why in the "expected vs. found" cell.

---

## Gap types

Grouped by where the fix belongs. Preview reports only what changes what renders, or what it can't show. Marks are described in `page-anatomy.md`, section 9: **fill** (hotpink on the missing property), **placeholder** (dashed double stroke with a label, in place of what's missing), **guessed** (the guess as is, with the dashed double stroke and a label), **label** (a hotpink text label beside the item).

### Fix in Figma (`figma` mode)

| Type | Example | Mark in preview |
|---|---|---|
| Node unreadable | `figma-nodes` points to a node that's missing, moved or not accessible | Placeholder, labeled with the node ID |
| Empty variable | A variable or style with no value in a mode | Fill, or the node's literal with a label |
| State missing in Figma | The spec names a state or variant Figma doesn't have | The default, with a "state missing" label |
| Instance diverges | An instance in a screen overrides a visual property of its main component (gradient, color, size) | Label on the instance |
| Unspecced component | Figma uses a component (inside a referenced node, or as a library instance on a screen) that no spec covers and no spec cites. If a spec cites it, it's "Missing spec" instead. Log each component once | Listed in hotpink under "Components used"; rendered from the node data |
| Unmigrated token | Mid-migration: a token name Figma doesn't define, rendered from its `tokens.md` value | Label on the Foundations entry |

Figma hygiene issues are checked by `design-spec-audit`.

### Fix in the specs (via `design-spec-authoring`)

| Type | Example | Mark in preview |
|---|---|---|
| Missing token | A token referenced in a spec isn't in `tokens.md` or Figma. If `tokens.md` itself is missing, this is one gap for the set, not one per token | Fill |
| Literal value | A visual styling value written in a spec instead of a token (`spec-only`), or any visual styling value in a `figma`-mode spec. Motion values and behavioral constants aren't literal values (`../design-spec-authoring/references/source-of-truth.md`, "Styling values vs. behavioral constants"). Values inside a duplicated `## Visual` section count only as Duplicated ownership | Label on the Structure table row |
| Undescribed behavior | Behavior the visuals imply but the spec doesn't state (a hover that scales, a card that is selectable) | Label on the Structure table note |
| Missing spec | A spec cites a spec name that doesn't exist or is `deprecated` (a broken reference), in either mode | The reference in spec content; a placeholder where it would render |
| Ambiguous visual | Relative or vague visual language in `## Visual` (`spec-only`), or a library-backed part that the theme tokens, Library block and overrides don't define (`spec-only-rendering.md`, "Library-backed components") | Guessed |
| Missing Visual section | A `spec-only` ui, feature, scene or screen spec, or `layouts.md`, without `## Visual`. A library-backed ui spec with no overrides needs none | Label ("no Visual section") |
| Duplicated ownership | A spec with both `figma-nodes` and `## Visual` | Label in the page header |
| Missing transform | `tokens.md` doesn't document the Figma-to-CSS name transform | Chat only |
| Stale version | `figma-version` doesn't match the current Figma file version (only when both are known) | Chat only; the header shows the mismatch |
| No visual change | `spec-only`: a state the spec names whose `## Visual` delta is missing, so it renders identical to default. In `figma` mode the same problem is "State missing in Figma" | The default, with a "no visual change" label |
| Unresolved motion | A Motion cell that names a default not in `motion.defaults` (or one name in a `+` combination isn't), or is blank, TBD, a value or not one of the allowed forms: rendered as an instant change. Also a runtime variant whose "Differs by" cell names no motion, or a `Library:` cell the Source line doesn't map: rendered with Fallback, marked guessed | Label on the Motion cell; "guessed" where a default was chosen |
| Unresolved example | A worked example (in a host's Generated content or a component's Generative use) that uses a component that isn't AI-ready, isn't allowed in that host, or has no spec, or sample data that breaks its data contract | Placeholder |
| Missing Motion column | A ui, feature or scene component or screen whose States table has no Motion column. Its state changes render instantly | Chat only |
| Motion pages missing | `design-system/motion/` is empty or missing pages from its `pages` field (a set from before motion). State changes render instantly, and the Motion page and motion tokens are left out. Suggest adding them in `design-spec-authoring` (once per run) | Chat only |

### Can't be shown or checked

| Type | Example | Mark in preview |
|---|---|---|
| Couldn't read | Something the preview needed from a source but couldn't read, even with a read-only script: a collection `tokens.md` lists, or a mode (once), a page on the documentation platform | Fill or placeholder where it's used |
| Couldn't read (aliases) | The read returned only resolved values, so which primitive each semantic maps to isn't known (once) | Label on the Color page |
| Asset unavailable | An asset pointer that doesn't resolve (no file at its location, or an empty file), or a Figma asset that couldn't be downloaded or exported for a reason other than the environment | Placeholder, labeled with the layer or asset name |
| Contrast | A text-and-surface pair, as rendered, below WCAG 2.2 AA (4.5:1 for text, 3:1 for large text and UI parts) | Chat only; the Color page shows the ratio |

Contrast is reported as noticed, not audited, in every theme or brand mode the tokens define. A full accessibility review is recommended separately.

**Not gaps:** scene content (3D, video, interactive animation) isn't rendered by design; the overview key says so (`page-anatomy.md`, section 3). System-only flow steps have no screen by design. Figma motion data is ignored by design: motion comes from `motion/`. Ready for dev decides the Buildable badge, and is a setup limit when it can't be read; a mismatch with `status` is for the audit.

**Outside these types.** Preview isn't the audit. Spec issues that don't affect what renders (structure, misplaced acceptance criteria, naming) aren't reported item by item; if you notice any, add one line saying so, to raise in `design-spec-authoring` (`design-spec-audit` checks them in full).

---

## Setup limits

Some things can't be shown because of the environment, not the design: Ready for dev without the REST API, and the file version when the read method doesn't return it, fonts loaded by link or replaced by system fallbacks, and asset downloads blocked by the environment when the script export also failed (when the export worked, nothing is missing). These aren't gaps: they aren't counted in the totals and need no fix in Figma or the specs. They use the neutral mark, never hotpink (`page-anatomy.md`, section 9). List them once, after the gap tables, under **Not available in this setup**, one line each, with what would make it available (for example "Ready for dev: needs Figma's REST API with a personal access token in the environment"). Never ask the designer to fix their setup; it's optional.

---

## Reporting in chat

After presenting the preview:

**No gaps:**

> No gaps found. Everything in scope rendered from its source.

**Gaps found:**

1. One line with the total and the split: "14 gaps: 6 to fix in Figma, 5 in the specs, 3 that the preview can't show or check."
2. The three groups above, in that order, each as a short table: spec (or token) and location, type, expected vs. found, what the preview did.
3. **Not available in this setup**, if anything applies ("Setup limits" above).
4. Close with:

> Every hotpink mark in the preview is one of the gaps above; gaps marked "chat only" have no mark, and grey "unknown" marks are setup limits. Fix spec gaps in a `design-spec-authoring` session and Figma gaps in Figma, then re-run the preview to confirm they're closed. Run `design-spec-audit` to track spec issues formally in the audit register.

Keep the report factual and short. Don't restate items the designer can see in the preview unless they're gaps.
