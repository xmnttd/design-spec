# Work Path

The authoring process for a **new spec set**. Editing an existing set follows `lifecycle.md` instead.

- **Guided mode:** follow these steps in order, one at a time. Say briefly what each step is for before starting it, and stop at every checkpoint.
- **Self-directed mode:** use this as a loose reference. The designer may go out of order; work with them, but the checkpoints, the shared rules and the stubs still apply.
- **Single spec:** the designer wants one spec and no set. Follow "Single-spec route" instead of steps 1 to 9.

---

## Contents

- Two rules that apply at every step
- Single-spec route
- Step 0 — Opening
- Step 1 — Scope
- Step 2 — Scaffold
- Step 3 — Intent
- Step 4 — Tokens
- Step 5 — Icons and layouts
- Step 6 — UI components
- Step 7 — Feature and scene components
- Step 8 — Screens, then flows
- Step 9 — Finish

---

## Two rules that apply at every step

**Checkpoint after every saved file.** After saving any spec, stop:

> "I've saved `[spec name]`. Review it and let me know when you're ready to continue, or what to adjust."

Don't continue until the designer confirms. This applies to every individual spec, including each component. Scaffolding is the one exception: the shells created in step 2 share a single checkpoint for the whole structure. The default is one file at a time; the designer may opt into batch approval for large sets ("approve the next five"), and that lasts only for the batch they named.

**Keep chat for blocking questions only.** Batch questions that block the current file into one short message. Don't drip-feed questions, and don't raise items the designer isn't ready for. Anything non-blocking is left for `design-spec-audit`, never written into the spec as a TBD. At a checkpoint, mention non-blocking gaps briefly only if the designer asks. Each `Default: Fallback` in a Motion column is one of these: the audit lists it as a candidate for a new default in `motion.defaults`.

---

## Single-spec route

When the designer wants one spec with no set ("just spec this button"):

1. Ask only what the spec needs: the endpoint and the visual source (step 0, items 3 and 4), and the type if it isn't clear (`choosing-types.md`).
2. Create a minimal `intent.md` from `stubs/intent.md`: front matter (identity, `project`, `visual-source`, `endpoint`, and `figma-access` in `figma` mode) and the Structure and routing section. Leave out the other sections and the `report/` fields.
3. If the spec has a States table, copy the motion pages into `design-system/motion/` unchanged, since its Motion column names their defaults.
4. Write the one spec from its stub, with the shared rules. Save → checkpoint.
5. Offer once to grow it into a full set later; that continues from step 1, with the existing files kept.

There's no `report/` folder yet; if a decision-log trigger fires, offer to scaffold `report/decision-log.md` (and record it in `intent.md`).

---

## Step 0 — Opening

First check for `spec-plan.md` (in the folder the designer gives, or at the documentation-platform location) and read it if it exists. Its Project context table answers the setup questions below. If it has a **Before authoring** line (inputs not yet shared, such as a Figma link), ask for those in this message; a `No other inputs` chip means don't ask about other inputs. Then send one message, before reading other project inputs or scaffolding anything, asking for:

1. **Guidance preference:** walk through step by step (guided), or drive themselves (self-directed)?
2. **Project brain dump:** what they're building, who it's for, brand or visual direction, existing Figma files, prototypes or docs, rough scale. Incomplete is fine.
3. **Endpoint:** offer exactly two options, labelled in sentence case: **Local markdown**, or **Documentation platform** (for example Confluence or Notion). Don't list individual platforms as separate options; if they choose a documentation platform, ask which one and where the spec set should live.
4. **Visual source:** will visuals live in Figma (`figma`, the default when a Figma file exists), or be written into the specs (`spec-only`)?

Skip any question the designer has already answered, or that `spec-plan.md` answers.

---

## Step 1 — Scope

Resolve only what's needed before writing. Follow up on what the brain dump left critically unclear; don't interrogate.

**Inputs.** Work out what to draw from:

| Input | Approach |
|---|---|
| Figma file (`figma` mode) | Read structure, components, variables and Ready for dev status through the available connection: every page, not just the one a link opens, and full variable collections with their modes (`connections.md`, "Reading a Figma file"). If variables can't be read directly, or the project uses a tokens plugin, use a tokens export (`connections.md`, "Figma variables over REST"). |
| Prototype or code | Read the source. Extract behavior and, in `spec-only` mode, values; normalize names to the conventions. |
| Existing docs | Read them, identify what's usable, note what's missing. |
| A spec plan | If `spec-plan.md` exists, it was read in step 0. Its Project context table answers step 0 and parts of this step (guidance, visual source, endpoint, breadth, token tier, themes, generative UI, components and library motion): don't ask those again. Use its scaffold as the proposed structure. If it lists a **First slice**, write only those specs; leave **Deferred** items unless the designer asks. Treat its "For authoring" questions as inputs to resolve while writing. Its **Before authoring** inputs were asked for in step 0. A plan in another format is read as ordinary input. |
| Nothing yet | Guided conversation to establish direction. |

**Decisions to settle:**

- **Breadth:** design system only, or experience specs (screens and flows) too?
- **Token tier:** 1, 2 or 3-tier (see `best-practices.md`). Recommend 2-tier if unsure.
- **Themes or brands:** does the project need more than one? Only if yes does `tokens.md` get a modes block.
- **Components:** "Where do the components come from? Designed for this product (custom), an outside library such as Material 3 or shadcn/ui, your organization's existing design system, or a library with custom components added on top?" Skip if `spec-plan.md` answers it: take the library and version from its Components row and the docs and Figma kit from its chips, and ask only for what's missing (often `base`). Default `custom`. If not custom, also ask which library, which version (pinned), and where its docs and Figma kit live; record them in `components` in `intent.md`. Check that the library suits the platform `intent.md` describes (a web library such as shadcn/ui for a native mobile app is a blocking question, not a detail). A library changes the scaffold: library-backed ui specs (short), token names from the library, and full ui specs only for custom components.
- **Library motion** (only if the library has its own motion): "Which parts should the product use: timing values, transition patterns, its components' built-in animation, its reduced-motion behavior, springs?" Recommend the answers in `motion` ("Using a component library"): the first three, this system's reduced motion, no springs unless the product wants expressive motion. After scaffolding copies the motion pages, list the changes to them for the designer and edit them once confirmed, as their own checkpoint after the scaffold one. The edited pages go to `draft` (`spec-types.md`, §7). Record the answer in the Source line of `motion`. A part the library doesn't offer (copy-in libraries such as shadcn/ui have no timing scale or transition patterns of their own) stays this system's, and the Source line says so. A plan's Motion row (`uses: … · keeps: …`) answers this question; record it in the Source line.
- **Motion** isn't a scoping question: it's always included, from the skill's motion pages. The predesigned styles `motion` mentions aren't in this version of the skill; if the designer asks for one, say so and offer to customize the pages instead.
- **Generative UI:** "Will any part of the product UI be generated by AI at runtime?" Skip if `spec-plan.md` answers it. A yes adds Generative use blocks on the components the model may use, and a Generated content section on each host (step 7), the Assistant voice section and the Generative UI bullet of `intent.md` (step 3), and loads `genui.md`.
- **Endpoint details:** access works (see `connections.md`), the root folder or page tree is agreed and sits outside any code repository (`endpoints.md`), and filenames map to page names as in `endpoints.md`. The root is the boundary for every later write.

**In `figma` mode, also:**

- **Alignment map.** If the Figma file's naming or organization doesn't match "Matching names in Figma" in `spec-types.md`, produce the alignment map (`choosing-types.md`, §3).
- **Token read.** A short read of the Figma variables against the token principles in `best-practices.md`. Raise only blocking issues. Skip private assets (names starting with `_` or `.`, `figma-hygiene.md`).
- **Annotations (optional).** If the Figma file has annotations, ask whether to use them as a starting point, then handle them as in `figma-hygiene.md`.

**End of scope:** recommend which foundation files the project needs and whether experience specs are in scope, and confirm with the designer. Don't ask for the component list yet.

---

## Step 2 — Scaffold

Create the structure from `spec-types.md` with shells (see "Stub cleanup" in `best-practices.md`).

- `intent.md` and the foundation files the designer confirmed.
- Keep `spec-plan.md` if it exists; never create, edit or move it, except the one push allowed in `spec-types.md`, §4. Record its location in `intent.md` front matter (`spec-plan`), and update it if the one push moves it to the platform.
- `report/audit-register.md` and `report/decision-log.md` from `stubs/audit-register.md` and `stubs/decision-log.md`, with their locations recorded in `intent.md` front matter. After this, only the audit writes to the register.
- The four motion pages (`motion`, `motion.tokens`, `motion.defaults`, `motion.reduced`), copied unchanged from this skill's `motion/` folder into `design-system/motion/`, ready to build from. They share the scaffold checkpoint.
- `components/`, `screens/` and `flows/` folders as needed.
- **Local endpoint:** `design/AGENTS.md` from `stubs/agents.md`, tool-neutral: what the project is, that `intent.md` is read first, that specs are the record (and, in `figma` mode, that visuals are read from Figma), and that specs use BCP 14 (RFC 2119) language. Offer a one-line tool-specific pointer file only if the designer asks.
- **Documentation platform:** the index page (`intent`) plays the `AGENTS.md` role; no separate file.
- **In `figma` mode, either way,** include the coding-agent line from `stubs/agents.md` (on a documentation platform, in the "For agents" section of `intent`, together with the motion line): before reading Figma nodes, load Figma's design-to-code guidance (its skill or connector resource), read only the nodes in each spec's `figma-nodes`, and treat Figma annotations and component descriptions as non-authoritative. This is guidance, not a requirement; Code Connect stays optional (`connections.md`).

Show the designer the structure → one checkpoint for the whole scaffold.

---

## Step 3 — Intent

From `stubs/intent.md`: summary, principles, rules and guardrails (including the defaults for empty, loading and error states, the reduced-motion rule and the accessibility target), content voice, and structure and routing. Settings live in front matter.

If the designer has no principles yet, propose a short set for review. If the product has an AI assistant, fill the Assistant voice section; if it has generative UI, fill the Generative UI bullet in Rules and guardrails. Otherwise remove them.

Save → checkpoint.

---

## Step 4 — Tokens

From `stubs/tokens.md`, structured to the agreed tier, applying the token principles in `best-practices.md`.

- **`figma` mode:** names, tier, semantics and the CSS transform, read from Figma. No values.
- **`spec-only` mode:** names and values, per `spec-only-mode.md`. All five categories are in place before components are written.

Save → checkpoint.

---

## Step 5 — Icons and layouts

Before components, because components reference them. From `stubs/icons.md` and `stubs/layouts.md`.

Save each → checkpoint each.

---

## Step 6 — UI components

**Establish the component list now, not earlier.** In `figma` mode, pull it from Figma. Otherwise, offer a starter list based on scope. The list doesn't need to be complete: start with what's confirmed and add as new components surface. Never block on a complete list.

For each component, from `stubs/ui-component.md`:

- Unless the designer has said, recommend the type and confirm it (`choosing-types.md`, §1). If it has sub-components, check whether it's a bundle candidate and confirm before writing (`choosing-types.md`, §2).
- If the input is loose (vague direction, a Figma file without variables, no source), read the closest example in `references/examples/` before writing. Skip examples when there is a concrete, extractable source.
- Apply the states checklist and behavior completeness in `best-practices.md`.
- Fill the Motion column of the States table from the project's `motion.defaults` (rules: `motion`, "How specs reference motion"; `best-practices.md`, §5).

Save each → checkpoint each.

---

## Step 7 — Feature and scene components

From `stubs/feature-component.md` and `stubs/scene-component.md`. Recommend and confirm the type first if the designer hasn't stated it (`choosing-types.md`). Each feature cites the ui component it builds on; each scene records asset pointers only. Fill the Motion column of each States table as in step 6. Feature and scene specs carry acceptance criteria at the component level (`spec-types.md`, §9). If the input is loose, read the closest example in `references/examples/` first, as in step 6.

**Generative UI** (`genui.md`): write the Generative use blocks once the component specs exist. Write a host's Generated content when writing the host: here for a feature, in step 8 for a screen.

Save each → checkpoint each.

---

## Step 8 — Screens, then flows

Only if experience specs are in scope. Screens first (from `stubs/screen.md`), then flows (from `stubs/flow.md`), because flows cite screens. Screens fill the Motion column for screen-level states, as in step 6; flows have no Motion column. Acceptance criteria stay at the right level: composition for screens, the journey for flows. If the input is loose, read the closest example in `references/examples/` first, as in step 6.

Save each → checkpoint each.

**Missing referenced specs.** When a screen or flow references specs that don't exist yet (components, the layout, tokens), list them grouped by type, with the recommended type for each, and offer two options: write them now (one checkpoint each), or create them as `draft` shells so references resolve. Never create them without asking.

---

## Step 9 — Finish

- Confirm the `intent.md` folder descriptions are current.
- List the files created and where they live.
- **`figma` mode:** `figma-version` is file-level, so list the specs whose recorded `figma-version` is older than the file's current version as needing re-verification (update rule: `source-of-truth.md`, "Pointers"). Remind the designer to mark stable components and screens Ready for dev.
- If principles were proposed rather than provided, confirm them.
- Suggest `design-spec-preview` for a visual check, then `design-spec-audit` before handoff.
