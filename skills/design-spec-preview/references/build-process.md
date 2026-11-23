# Build Process

How a preview run goes, from finding the spec set to presenting the result. Page content is in `page-anatomy.md`; gap handling in `gap-report.md`.

---

## Environment

- **Prefer a fresh session** with no authoring context. The preview should see only what engineering will receive: the specs and, in `figma` mode, Figma.
- If the designer gives verbal guidance instead of specs ("the button should be blue"), don't render it. Say it belongs in the spec, via `design-spec-authoring`, and continue with what the specs say.

---

## Step 1 — Find and read the spec set

**Don't assume a folder name.** Look for `intent.md`, then the folders it routes to (`design-system/`, `experience/`, `report/`, see `../design-spec-authoring/references/spec-types.md`). Ask if there is more than one candidate. The folder holding `intent.md` is the **spec root**, whatever it's called.

**No `intent.md`:** stop. There is no spec set to preview. Say so, ask whether the spec set lives somewhere else, and otherwise suggest `design-spec-authoring` to create one. Don't build from loose files or from Figma alone.

Then:

1. Read `intent.md` front matter: `project`, `visual-source`, `endpoint`, `figma-access`. `endpoint` says where the specs live:
   - `local`: read the files.
   - `documentation platform: <name>, <root>`: read the pages under that root, through the platform's connector or API (`../design-spec-authoring/references/connections.md`). Front matter fields are page properties (`../design-spec-authoring/references/endpoints.md`).
2. Read every spec in scope, front matter first. Skip `report/`, `spec-plan.md`, `AGENTS.md` and any `README.md`: they aren't specs and aren't previewed.
3. Note each spec's `status`. `deprecated` specs are listed in the overview but not rendered.
4. If `tokens.md` is missing, warn once and continue; components render unstyled with their gaps marked, and it's one gap (`gap-report.md`, "Missing token").
5. Read the motion pages in `design-system/motion/`. If they are missing, the Motion page and motion tokens are left out (`gap-report.md`, "Motion pages missing").

---

## Step 2 — Scope and build mode

One message, after reading, covering:

- **What was found:** counts by type (tokens, icons, ui, feature, scene, screens, flows), the visual source, and anything blocking (Figma unreachable, an `export` read method with no nodes, missing `tokens.md`).
- **Scope:** the whole set (default), or named specs. A partial preview always includes what the named specs depend on, followed recursively: `intent.md`; the tokens and the motion pages; every screen a flow passes through; every component and the layout a screen cites; the ui component a feature builds on; a host's allowed components (and the AI-ready components nested in them); a bundle's parts; and the icons any of them use.
- **Re-run:** if `design-preview/` already exists, say it will be regenerated (in the opening line of the final reply, if no message is sent). This alone is no reason to wait for an answer.
- **Build mode:**
  - **Auto-run** (default): builds the whole preview without stopping, then presents it.
  - **Step-by-step:** pauses after each phase so the designer can review before continuing.
- **Figma setup** (`figma` mode only), only if something needs a decision: the design-to-code offer and the Code Connect question (`figma-mode.md`, section 1).

Skip anything the designer already answered; if build mode wasn't chosen, use auto-run. If nothing is left to ask and nothing blocks, don't send the message: build, and open the final chat reply with the "what was found" line instead. Otherwise wait for the answer before building.

---

## Step 3 — Output location

- A folder named `design-preview/`, **next to** the spec root (a sibling of the folder holding `intent.md`), never inside it. For a documentation-platform spec set, create it in the local working folder; it is never pushed to the platform.
- **Never inside a code repository.** Before creating the folder, check whether the location or a parent folder contains `.git`. If it does, say so and agree a location outside it (`../design-spec-authoring/references/endpoints.md`, "Outside code repositories").
- Contents: `index.html` and `assets/` (downloaded images, icons, fonts). No other files.
- A re-run regenerates the folder (step 2, "Re-run").
- The preview is for the designer. It isn't the handoff artifact and isn't shared with engineering.

---

## Step 4 — Build rules (both modes)

- **Never write the whole file in one call.** One write creates the shell (150 lines or fewer, like every edit); then one edit per page or part of a page. Component CSS goes in `<style>` blocks in `<head>` (one per component, added by its own edit), never inside the page body.
- **No single edit over 150 lines.** Split longer pages: interactive demo first, then states, then structure, then spec content. Long spec content (a large `tokens.md`) is itself split across as many edits as it needs, and so is a large block of token custom properties in the shell.
- **Don't delegate file generation to a subagent.** Subagent file output can time out with nothing recoverable. Write the file directly.
- **Keep the file valid after every edit,** so it opens at any point.
- **Self-contained:** inline CSS and JS, including the inline helpers (`page-anatomy.md`, section 10); assets from the local `assets/` folder only. No external scripts, stylesheets or temporary asset URLs, and no `fetch`, module imports or other requests at runtime, so the page works opened directly from `file://`. The one exception is one font stylesheet link and the font files it loads (Fonts, below).
- **Fonts,** in this order:
  1. Font files the spec set points to (`tokens.md` font sourcing), copied into `assets/`.
  2. Files downloaded from a public font service (for example Google Fonts) during the build, copied into `assets/`.
  3. If downloads are blocked and the font is on a public font service: one stylesheet link to that service in `<head>`, plus `local()` in `@font-face` so a copy installed on the viewer's computer is used. The viewer's browser loads it when the page opens; offline, it falls back.
  4. Otherwise, system fallbacks.
  Always end the font stack with system fallbacks. Note the source on the Typography page. Options 3 and 4 are setup limits, listed once in the setup note (`gap-report.md`, "Setup limits").

---

## Step 5 — Phases

| Phase | Builds | Step-by-step pause |
|---|---|---|
| 1. Shell | Document, chrome styles, all token custom properties (with theme or brand modes), the motion tokens from `motion.tokens` (if the motion pages exist), nav, empty page containers, page switching, theme control, motion-level control; then, in their own edit, the copy generator and markdown renderer (`page-anatomy.md`, section 10) | Confirm the nav structure |
| 2. Motion helper | One implementation per `motion.defaults` row (`page-anatomy.md`, section 10). Skipped if the motion pages don't exist | — |
| 3. Overview | The overview page | — |
| 4. Foundations | One edit each: color, typography, spacing, elevation, shape and size, icons, layouts, motion (the helper's showcase) | After all foundations |
| 5. Components | One component per edit (more for complex ones): ui first, then feature, then scene; a feature host after the components it allows, since its worked examples reuse their templates | After each component |
| 6. Screens | One screen per edit | After all screens |
| 7. Flows | One flow per edit | After all flows |
| 8. Final | Verify, present, report gaps | — |

**Auto-run** saves after each edit and continues without stopping. **Step-by-step** shows the file at each pause and waits for the designer.

---

## Step 6 — Verify, present, report

1. **Verify** before presenting:
   - Every page in the nav has content.
   - Switch the motion-level control through Full, Reduced and None on a component page: under Reduced, travel is removed, fades run at the reduced duration and state changes stay visible; under None, every change is instant and still visible.
   - **Motion end state (MOT-12)** is guaranteed by construction (`page-anatomy.md`, section 10): no `forwards` fill and no inline styles left behind. When a browser is available, spot-check one demo per default used: once it finishes, it looks as it does with motion off.
   - A sample of rendered spec tables shows no stray backticks, asterisks or brackets.
   - No temporary asset URLs remain; every asset reference points to a non-empty file in `assets/`.
   - In `figma` mode, compare each rebuilt node with its design-to-code screenshot (or the node data, if design-to-code isn't in use). Fix mismatches you caused. Mismatches that come from Figma or the specs are gaps, not fixes.
   - No runtime requests (`fetch`, module imports, external URLs) other than one font stylesheet link and the font files it loads, so the page opens from `file://`.
   - If you can render the page (a browser or preview tool), look at each page. If you can't, check structure only (valid HTML, scripts parse, every link resolves) and say in the chat reply that the preview wasn't visually checked.
2. **Present** the file: in an app that can show files, present `design-preview/index.html`; otherwise give the path to open in a browser.
3. **Report gaps in chat** (`gap-report.md`).
