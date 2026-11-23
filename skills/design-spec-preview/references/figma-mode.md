# Figma Mode

How preview reads Figma and rebuilds what it reads as live HTML/CSS. Applies when `visual-source: figma`, and to any spec with `figma-nodes` during a migration. Ownership rules are in `../design-spec-authoring/references/source-of-truth.md`.

---

## Contents

1. Before reading Figma
2. What to read
3. Reading efficiently
4. Tokens from variables
5. Rebuilding nodes
6. Assets
7. Multiple Figma files
8. When Figma can't be read

---

## 1. Before reading Figma

**Figma's design-to-code skill.** It defines the correct way to read nodes for implementation (screenshot as the visual target, adapting reference code, handling sparse responses, assets).

| Situation | Do |
|---|---|
| Installed | Use it silently. Load it before the first design-context read and follow it, as the method for reading nodes |
| Not installed | Suggest installing Figma's official plugin once, in the step 2 message: it gives a more faithful preview |
| Declined | Read node data directly through whatever connection works, and say once that fidelity may be lower |

When it's in use, follow it with these overrides:

- **Output:** plain HTML and CSS, using the token custom properties from section 4, with no framework and no Tailwind. Where a read tool takes a target language or framework, request HTML/CSS.
- **No project components:** its instructions on reusing "the project's" components or stack don't apply; there is no project codebase. Reuse only the preview's own builds (section 2).
- **Output location:** write only into `design-preview/` (`build-process.md`, step 3).
- **Screenshots** are the visual target for comparison only: never saved to `assets/` and never shown in the preview.

**Connection.** Detect what is connected before asking for anything (`../design-spec-authoring/references/connections.md`). Read access to the files in `figma-access` is enough:

- `files[].url`: each file to read.
- `files[].role`: `library`, `composition`, or `library and composition` (section 7).
- `files[].library-key`: the published library key, for scoping library searches (section 3).
- `read-method`: the method to use. Honour it when it's available; if it isn't, use what works and say so once. An `export` gives variables only: tokens come from the export, and nodes can't be read. Say so once, as a blocker in the step 2 message, as for Figma unreachable (section 8).

Preview never writes to Figma. The full rules are in `connections.md`:

- **Only these methods:** `connections.md`, "Only these methods".
- **Read-only scripts** (for variable modes, aliases, styles, asset exports): `connections.md`, "Read-only scripts are reads". Load Figma's skill for its script tool before the first script.
- **Code Connect:** relay the connector's question word for word once, and continue read-only (`connections.md`, "Code Connect").

---

## 2. What to read

Read only what the specs point to (who reads what across the family: `../design-spec-authoring/references/connections.md`, "Reading a Figma file"):

- Each spec's `figma-nodes`: the main component, component set, section or frame. Worked examples of generative UI have no nodes of their own: they render from the components' builds. **Read every node listed;** never skip one to save time. On screens, the list usually includes one frame per state and breakpoint, named `screen/<name>/<state>/<breakpoint>` (`../design-spec-authoring/references/spec-types.md`, §5), for example `screen/home/error/compact`. Build one responsive screen from the default frames and verify it against each breakpoint frame; apply each state frame as an override for that state in the switcher (section 3).
- What those nodes need to render: their variables, text styles, effect styles and assets.
- The variable collections `tokens.md` lists in `figma-nodes.collections` (section 4).
- Nested components inside a node are read as part of it, but rendered by reusing the preview's own build of that component (its spec) wherever the component is already in scope.

Never browse pages, sections or nodes that no spec references. If something a node needs is outside the specs (a nested component no spec covers or cites), render it from the node data and report it as an "Unspecced component" gap.

**Ignore as sources:** private assets (styles, variables, components or sections whose names start with `_` or `.`; `../design-spec-authoring/references/figma-hygiene.md`), Figma annotations, component descriptions, documentation links on components, any text layer that documents rather than designs (role text, spec-link cards, notes on the canvas), and motion data (section 5). Behavior and documentation come from the specs only.

**Read and show, but never copy into specs:** the Ready for dev status of each referenced node, and the file `version` or `lastModified`. Ready for dev comes from the REST API (`devStatus`); the file version from REST or any read method that returns it (`../design-spec-authoring/references/connections.md`, "File version unreadable"). When they can't be read they're setup limits, not gaps: show buildable and the version match as unknown, with the neutral mark (`page-anatomy.md`, section 9), and list them once in the setup note (`gap-report.md`, "Setup limits"). Compare `figma-version` in each spec with the current version of its file only when both are known; a mismatch means the spec hasn't been checked against the latest Figma, and is a "Stale version" gap.

---

## 3. Reading efficiently

- **Metadata first** for large frames: read the node's structure, then request design context for the children you need, in one parallel batch. A sparse or truncated response can't be used directly; follow up on its visible children.
- **Component sets in one read.** A component set returns every variant; use it for both the live demo and the forced states.
- **Screen state frames** mostly reuse what the default frame already shows: read each one as an override read (which components it holds, their variant or state, text), not a full rebuild, and send those reads as one parallel batch.
- **Repeated instances** (five cards in a grid): build the component once from its own spec page, then read each instance only for what it overrides (text, swapped nested instances, visibility, variant). Don't re-read the whole subtree per instance when a smaller read gives the overrides; send override reads as one parallel batch.
- **Deep artwork** (illustrations built from many vector or masked layers) is an asset, not UI: request it as one asset rather than rebuilding layer by layer (section 6). If the asset tool won't take an instance ID, request the artwork's own main component or frame.
- **Variable aliases:** if the read returns only resolved values, show those on the Color page and report "Couldn't read (aliases)" once.
- **Library searches** only to resolve a nested instance inside a referenced node, scoped to the file's `library-key` (from `figma-access`). Never search to discover content. An unscoped search can return other teams' libraries.

---

## 4. Tokens from variables

- Read the variables bound in the referenced nodes, with their collections and modes.
- **Listed collections:** read the collections `tokens.md` lists in `figma-nodes.collections`, with their modes, through a connector's variables tool, a read-only script or the REST variables endpoint. Read raw values, not resolved ones: a resolved value hides which primitive a semantic maps to (`connections.md`, "Aliases are in the raw values"). If a listed collection or mode can't be read, Foundations show only the bound variables for it, and "Couldn't read" is reported once.
- **Styles:** read the text and effect styles the read nodes use, skipping private assets. Foundations show what was read (`page-anatomy.md`, section 4).
- **Never derive a value** from another source, for example a primitive's value from a semantic's resolved color, or a Dark value from Light. Anything that can't be read renders hotpink and is reported.
- **Mid-migration**, Figma variables win over `tokens.md` values (`spec-only-rendering.md`, "Mid-migration").
- Emit them as CSS custom properties in `:root`, named by the transform documented in `tokens.md` (for example `color/text/primary` → `--color-text-primary`). If `tokens.md` documents no transform, use slashes to hyphens with a `--` prefix, and report the missing transform as a gap.
- **Modes** (light and dark, brands) become `[data-theme="…"]` or `[data-brand="…"]` blocks overriding the semantic properties. The default mode goes in `:root`.
- **Text styles:** emit per the text-style rule (`page-anatomy.md`, section 4).
- **Effect styles** that aren't variables: emit as grouped properties or a utility class per style, named after the style.
- Note on the Foundations pages which entries are styles, not variables.
- **Unbound values** in a node (a hard-coded color, gradient or size): use the literal so the rebuild is faithful (binding is checked by the audit, FIG-9).
- **Empty or unresolvable variables:** render the literal from the node if there is one, otherwise hotpink, and report.
- Components and screens reference the custom properties, never the resolved values, wherever a variable exists.

---

## 5. Rebuilding nodes

Treat returned reference code as a high-fidelity, non-interactive prototype. Rebuild it as:

- **Semantic HTML:** native `button`, `a`, `input`, `dialog`, lists and headings, with roles and accessible names from the spec's Accessibility section.
- **Real layout:** flex and grid that match the design, instead of absolute positioning, unless the design can only be expressed with fixed positioning (overlapping artwork, canvases).
- **Real interaction:** hover, active, focus and disabled through real pseudo-classes and attributes; other states (selected, open, error, loading) through the spec's triggers. Variants that differ only by state become CSS state rules, not separate markup.
- **Derived behavior:** when a variant is a transform of the default (a hover that is the default scaled 1.1× with a shadow), implement it as that transform, and note in the component's Structure table how it was derived so the designer can confirm it. If the spec doesn't state the behavior, that's a gap.
- **Focus** follows the focus rule in `page-anatomy.md`, section 5. Reproduce Figma's ring position (inside or outside).
- **Motion** comes from the project's `design-system/motion/` and each spec's Motion column, played through the motion helper (`page-anatomy.md`, sections 5 and 10), never from Figma. Figma's motion data (prototype transitions, smart animate, animation timing) is ignored, and so are motion variables if Figma mirrors the tokens: `motion.tokens` wins. Never invent durations or easings.
- **Content:** placeholder text from the component, or instance text for screens. Never invent content.

A state or variant the spec names but Figma lacks: don't invent its look. Show it as a missing state (`page-anatomy.md`, section 5) and report "State missing in Figma".

---

## 6. Assets

- Download every image, icon, illustration and logo the rendered nodes use into `assets/`, following the connector's or design-to-code's asset instructions. Use each asset at its exact position and size. Never redraw, extract paths from, inline-edit or substitute an asset.
- Keep data-driven imagery dynamic in demos (a slot that takes any image), and use the Figma asset as its sample.
- **Scene posters** come from the scene spec's `assets:` pointer, not from Figma: copy the file into `assets/` if its location is reachable; otherwise show a missing-asset placeholder and report "Asset unavailable".
- **Fonts:** see `build-process.md`, step 4.
- **If downloads are blocked** (the environment refuses Figma's asset URLs, or they've expired), export from Figma instead with a read-only script: icons, logos and vector artwork as SVG (`exportAsync` with `SVG_STRING`), images as PNG. Save each export into `assets/`. This is Figma's own export, not a redraw. Only if the export also fails: render a placeholder at the exact size and position, labeled with the layer name. If the environment blocked both, it's a setup limit with the neutral mark; otherwise it's an "Asset unavailable" gap with the hotpink placeholder (`page-anatomy.md`, section 9). Report each asset once. Never fall back to a screenshot of the node.
- Figma's asset URLs are temporary: download the files, and never reference the URLs in the output (`build-process.md`, step 4).

---

## 7. Multiple Figma files

A project may keep its library (components and variables) in one file and its compositions (screens) in another, or both in one file.

- `figma-access.files` lists each file. Where a file has a `role` (`library`, `composition`, or `library and composition`), use it; otherwise infer it from what the specs point at.
- Each spec's `figma-nodes.file` says which file to read for that spec.
- Variables come from the library file that `tokens.md` points at. Screens in a composition file use library instances; render them by reusing the preview's build of each library component, applying only instance overrides.
- `figma-version` is per file: compare each spec against the version of the file it points to.
- A screen using a library component that no spec covers or cites is an "Unspecced component" gap.

---

## 8. When Figma can't be read

The general rule is in `gap-report.md`, "Collecting gaps". For Figma:

- **Figma unreachable:** say so up front in the step 2 message. Ask whether to continue: everything that depends on Figma renders as labeled placeholders, and behavior, states and acceptance criteria still render from the specs. Never present a partial preview as complete.
- **A node can't be read** (deleted, moved, no access): placeholder at the spec's page, labeled with the node ID, and a gap. For a screen state frame, the state may instead be composed from the spec (its components in the states the spec names), marked with the dashed hotpink outline and the label "Built from the spec, not read from Figma". Never show it unmarked.
- Never switch to `spec-only` rendering because Figma is unreachable, and never fill values from memory or from `tokens.md` names alone.
