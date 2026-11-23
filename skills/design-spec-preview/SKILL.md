---
name: design-spec-preview
description: "Builds a browsable, interactive HTML preview of an existing design spec set (one with intent.md) so a designer can see and use the design before engineering handoff: tokens, icons, layouts, motion, ui, feature and scene components, generative UI (AI-ready components and hosts, from their worked examples), screens and flows. In figma mode it rebuilds the referenced Figma nodes as live HTML/CSS and pairs them with the specs' behavior; in spec-only mode it renders from tokens and each spec's Visual section. Reports gaps in chat. Use this whenever a designer wants to preview, visualize, render, see or click through their specs or design system, check that Figma and specs translate well, or do a visual check before handoff, even if they don't say \"preview\". Not for previewing a Figma design that has no specs, writing or editing specs (design-spec-authoring) or auditing them for drift and consistency (design-spec-audit).\n"
---

# Design Spec Preview

Builds a local, self-contained HTML preview of a spec set: every token, component, state, screen and flow the specs describe, rendered so the designer can see it and interact with it. It is the designer's visual check before handoff, and a dress rehearsal for what an engineering agent will build from the same inputs. Gaps come back in chat, never in the HTML.

This file is the summary and router. Each section points to the reference file that holds the full rule; when the two seem to differ, the reference file wins.

---

## Part of the design-spec plugin

This skill shares its rules with `design-spec-authoring` and reads them from that skill's folder. All paths in this skill are relative to this skill's folder, including those inside `references/`.

- `../design-spec-authoring/references/source-of-truth.md`: who owns each fact, by mode.
- `../design-spec-authoring/references/spec-types.md`: the core schema (types, names, front matter, folders).
- `../design-spec-authoring/references/spec-only-mode.md`: how visuals are written in `spec-only` mode.
- `../design-spec-authoring/references/connections.md`: reaching Figma and documentation platforms.
- `../design-spec-authoring/references/endpoints.md`: how specs map to documentation-platform pages.

**Before anything else,** confirm those files are readable. If they aren't, stop and tell the designer the design-spec plugin is incomplete: all of its skills must be installed together.

---

## Start here

1. **Find the spec set** and read `intent.md` (`references/build-process.md`, step 1). No `intent.md` means there is no spec set to preview: stop and suggest `design-spec-authoring`.
2. **Read `visual-source`** from `intent.md`. It decides where visuals come from:
   - `figma`: rebuild the referenced Figma nodes as live HTML/CSS (`references/figma-mode.md`).
   - `spec-only`: render from `tokens.md` values, `layouts.md` and each spec's `## Visual` section (`references/spec-only-rendering.md`).
   - Mid-migration (`figma` set, some specs still with `## Visual`): render each spec from the source it has, label which, and let Figma variables win (`references/spec-only-rendering.md`, "Mid-migration").
3. **Agree scope and build mode** in one message (`references/build-process.md`, step 2).
4. **Build incrementally** (`references/build-process.md`, steps 3–5), page by page, following `references/page-anatomy.md`.
5. **Verify and present the preview, then report gaps in chat** (`references/build-process.md`, step 6, and `references/gap-report.md`).

---

## What to load, and when

| File | Load when |
|---|---|
| `references/build-process.md` | Always |
| `references/page-anatomy.md` | Always, before writing the shell |
| `references/gap-report.md` | Always; collect gaps from the first read |
| `references/figma-mode.md` | `visual-source: figma`, or any spec has `figma-nodes` |
| `references/spec-only-rendering.md` | `visual-source: spec-only`, or any spec has a `## Visual` section |
| `../design-spec-authoring/references/source-of-truth.md` | Always |
| `../design-spec-authoring/references/spec-types.md` | Always (the core schema) |
| `../design-spec-authoring/references/spec-only-mode.md` | `visual-source: spec-only`, or any spec has a `## Visual` section |
| `../design-spec-authoring/references/figma-hygiene.md` | `visual-source: figma`, or any spec has `figma-nodes` (private assets) |
| `../design-spec-authoring/references/connections.md` | Connecting to Figma or a documentation platform, or a connection fails |
| `../design-spec-authoring/references/endpoints.md` | The spec set lives on a documentation platform |

---

## Non-negotiables

- **Read-only.** Preview never writes to specs, Figma, the audit register or the decision log, and uses script tools for read-only code only (`../design-spec-authoring/references/connections.md`, "Read-only scripts are reads"). Its only output is the preview folder (`references/build-process.md`, step 3).
- **Outside the spec tree.** The preview folder never sits inside the spec set, a code repository or the documentation platform.
- **Self-contained.** Inline CSS and JS, assets from `assets/` only, and no requests at runtime. The one exception is one font stylesheet link and the font files it loads (`references/build-process.md`, step 4).
- **Scoped reads.** In `figma` mode, read only the nodes the specs point to (`figma-nodes`) and what those nodes need, the variable collections `tokens.md` lists, and the styles the read nodes use. Never browse the file for extra content (`references/figma-mode.md`).
- **Live, not pictures.** Components and screens are rebuilt as interactive HTML/CSS. Screenshots or renders of UI never appear in the output; artwork (images, illustrations, logos, icons) appears only as downloaded assets (`references/figma-mode.md`).
- **Never fill gaps from memory.** A value, asset or node that can't be read is shown as a marked placeholder and reported. "Couldn't check" is never "checked" (`references/gap-report.md`).
- **Specs are the record.** Behavior, states, accessibility and acceptance criteria shown in the preview come from the specs, never from Figma annotations or component descriptions.
- **Never ask for credentials in chat** (`../design-spec-authoring/references/connections.md`).
- **Gaps in chat only,** not in the HTML (`references/gap-report.md`).

---

## The design-spec family

| Skill | Role |
|---|---|
| `design-spec-plan` | Optional. Plans a spec set before writing and saves `spec-plan.md` |
| `design-spec-authoring` | Writes and edits specs; holds the shared rules |
| `design-spec-preview` | Renders specs (with Figma in `figma` mode) as a local, interactive visual check (this skill) |
| `design-spec-audit` | Read-only QA; the only writer of the audit register |

Preview can run at any time. Suggest it after visuals are drafted or changed, and before handoff. Gaps it reports are fixed in `design-spec-authoring` or in Figma; `design-spec-audit` tracks findings formally in the audit register.
