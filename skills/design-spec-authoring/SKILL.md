---
name: design-spec-authoring
description: "Writes and maintains design specs for engineering handoff: foundations (tokens, icons, layouts, motion), ui, feature and scene components, generative UI (components marked AI-ready and hosts with generated content), screens and flows, as markdown files or documentation-platform pages, with or without Figma. Use whenever a designer wants to spec out, scaffold, write, update, rename or retire specs or a spec set, add acceptance criteria, fix Figma naming to match specs, or prepare specs a coding agent can build from, even if they don't say \"spec\". Prefer it over general handoff skills when the work produces spec files or pages. Not for planning a spec set (design-spec-plan), previewing (design-spec-preview) or auditing (design-spec-audit).\n"
---

# Design Spec Authoring

A companion for writing and maintaining design specs across a product's life, from the first scaffold through every later edit. Specs are written so that designers, engineers, QA and coding agents can all read them and arrive at the same build.

The core idea: **every fact has exactly one owner.** In `figma` mode, Figma owns how things look and the specs own everything else. In `spec-only` mode there is no Figma, and the specs own the visuals too. Nothing is stated in two places, because duplicated facts drift. Details: `references/source-of-truth.md`.

This file is the summary and router. Each section below points to the reference file that holds the full rule; when the two seem to differ, the reference file wins.

**Part of the design-spec plugin.** This skill holds the shared rules that `design-spec-preview`, `design-spec-audit` and `design-spec-plan` read from its folder. All its skills install together. If plan, preview or audit is missing (for example `../design-spec-preview/SKILL.md`), warn the designer once that the install is incomplete and should be reinstalled, then carry on as normal; authoring works on its own. This is the only rule about a missing skill.

---

## Start here

1. **Is there an existing spec set?** Look for `intent.md`, locally or at the documentation-platform location the designer gives.
   - **`intent.md` exists:** read it, ask what is changing, and follow `references/lifecycle.md`. Adding specs to an existing set goes this way too. If the designer names what to edit up front, go straight to `references/lifecycle.md`.
   - **Specs exist but no `intent.md`:** adopt them. Create a minimal `intent.md` (front matter and routing), then continue on the editing path (`references/lifecycle.md`, "Adopting existing specs").
   - **The designer wants one spec, with no set:** create a minimal `intent.md`, then write the one spec from its stub, skipping the full scaffold. Offer to grow it into a set later (`references/work-path.md`, "Single-spec route").
   - **Nothing yet:** new spec set. Follow `references/work-path.md`. If `spec-plan.md` exists (from `design-spec-plan`), read it first and skip what it already answers (`references/work-path.md`, steps 0 and 1).
2. **New spec set:** open with one message covering guidance preference, a project brain dump, the endpoint and the visual source (`references/work-path.md`, step 0).
3. **Before writing the first spec,** read the three shared rule files: `references/source-of-truth.md`, `references/spec-types.md`, `references/best-practices.md`.

---

## What to load, and when

Load only what the current task needs.

| File | Load when |
|---|---|
| `references/source-of-truth.md` | Always, before the first write |
| `references/spec-types.md` | Always, before the first write |
| `references/best-practices.md` | Always, before the first write |
| `references/choosing-types.md` | Choosing or confirming a spec type, checking for a bundle, or aligning Figma names |
| `references/work-path.md` | Creating a new spec set (guided mode: follow it strictly; self-directed: as a loose reference), or a single spec |
| `references/lifecycle.md` | Editing, adding to, renaming or retiring specs in an existing set |
| `references/spec-only-mode.md` | Only when `visual-source: spec-only` |
| `references/figma-hygiene.md` | `visual-source: figma` |
| `references/genui.md` | The product has generative UI or an AI assistant |
| `references/endpoints.md` | The endpoint is a documentation platform, or files are being created or moved |
| `references/connections.md` | Connecting to Figma or a documentation platform, or a connection fails |
| `stubs/<type>.md` | Writing or checking coverage of a spec of that type |
| `motion/` (this skill) | Scaffolding a new spec set, or adding motion to an existing one: copied unchanged into `design-system/motion/` |
| `design-system/motion/motion` and `motion.defaults` (the project's) | Filling a Motion column in a States table |
| `references/examples/` | Only when the input for a spec is loose (vague direction, Figma without variables, no source) |

---

## Spec types and stubs

Unless the designer has named the type, recommend one with a one-line reason and confirm it before writing; for components with sub-components, check whether they form a bundle and confirm that too (`references/choosing-types.md`). Then start from the type's stub.

| Type | Filename | Folder | Stub |
|---|---|---|---|
| intent | `intent.md` | `design/` | `stubs/intent.md` |
| foundation | `tokens.md`, `icons.md`, `layouts.md` | `design/design-system/` | `stubs/tokens.md`, `stubs/icons.md`, `stubs/layouts.md` |
| foundation: motion | `motion.md`, `motion.tokens.md`, `motion.defaults.md`, `motion.reduced.md` | `design/design-system/motion/` | None: finished pages in `motion/`, copied unchanged |
| ui-component | `ui.<name>.md` | `design/design-system/components/` | `stubs/ui-component.md` |
| feature-component | `feature.<name>.md` | `design/design-system/components/` | `stubs/feature-component.md` |
| scene-component | `scene.<name>.md` | `design/design-system/components/` | `stubs/scene-component.md` |
| screen | `screen.<name>.md` | `design/experience/screens/` | `stubs/screen.md` |
| flow | `flow.<name>.md` | `design/experience/flows/` | `stubs/flow.md` |

When `intent.md` names a component library (`components.source` isn't `custom`), ui components that come from it are **library-backed**: the same type and stub, using its Library block and leaving out what the library's docs own (`references/spec-types.md`, §2).

Generative UI is components marked AI-ready (a Generative use block) and hosts with a Generated content section, both in the stubs above (`references/genui.md`).

Bundles are for ui, feature and scene components: they add `.bundle` to the filename and use the bundle block in the matching stub (`stubs/ui-component.md`, `stubs/feature-component.md` or `stubs/scene-component.md`). Stub tags, shells and cleanup: `references/best-practices.md`, "Stub cleanup". The non-spec files scaffolded in work-path step 2 have stubs too: `stubs/agents.md`, `stubs/decision-log.md`, `stubs/audit-register.md`.

---

## Working with the designer

- **Checkpoint after every saved spec:** stop and wait for the designer to confirm before continuing, on both the creation and editing paths; scaffolding shares one checkpoint (`references/work-path.md`).
- **Chat is for blocking questions only,** batched into one message; everything else is left for the audit, and mentioned briefly at a checkpoint only if the designer asks (`references/work-path.md`).
- **Specs hold decisions only:** no TBDs, open questions, history or argument (`references/best-practices.md`, §1).
- **Designers edit by hand too:** read before writing, re-check the version before saving, merge rather than overwrite (`references/lifecycle.md`).

---

## Non-negotiables

- **One owner per fact:** no visual styling values in specs in `figma` mode; in `spec-only` mode, styling values only in `tokens.md` and layout values as `references/spec-only-mode.md`, §1 says. Motion values live only in `motion.tokens` (`references/source-of-truth.md`).
- **`motion/` is copied unchanged at scaffold,** including its `ready` status. Afterward it is edited like any other spec, only when the designer asks (`references/lifecycle.md`).
- **Never restate a component library's docs** (`references/source-of-truth.md`, "Component libraries").
- **Specs live outside code repositories,** in their own folder or on a documentation platform (`references/endpoints.md`).
- **Stay in scope:** only the specs the designer named, inside the agreed root; confirm before touching others (`references/lifecycle.md`).
- **No Figma writes without explicit approval;** where Figma's official plugin is installed, follow its skills for how (`references/connections.md`).
- **Never write to the audit register;** authoring only scaffolds its empty page (`references/lifecycle.md`).
- **Never choose `in-review`, `ready` or `implemented` yourself.** Status on edit: `references/spec-types.md`, §7.
- **Never ask for credentials in chat** (`references/connections.md`).
- **"Couldn't check" is never "checked";** never fill gaps from memory (`references/connections.md`).

---

## The design-spec family

| Skill | Role |
|---|---|
| `design-spec-plan` | Optional. Plans a spec set before writing and saves `spec-plan.md` |
| `design-spec-authoring` | Writes and edits specs; holds the shared rules (this skill) |
| `design-spec-preview` | Renders specs (with Figma in `figma` mode) as a local, interactive visual check |
| `design-spec-audit` | Read-only QA; the only writer of the audit register |

The typical order is plan, author, preview, audit, but it isn't a required sequence. Authoring is the usual starting point and works without a plan; preview can run at any time. Suggest `design-spec-preview` after visuals are drafted or changed (especially in `spec-only` mode, where it is the only way to see the design). Suggest `design-spec-audit` before handoff and after significant changes.
