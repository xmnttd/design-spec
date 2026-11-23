# Scaffold Guidance

How to derive the scaffold shown in the plan. The plan **recommends** the structure; `design-spec-authoring` creates the empty shells.

## Source of truth

Authoring owns spec types, folders and naming. Read them there; don't restate them in the plan:

- `../design-spec-authoring/references/spec-types.md`: section 4 (folder structure) for the tree, and the spec types and naming rules for filenames.
- `../design-spec-authoring/references/work-path.md`: step 1 ("End of scope") and step 2 (scaffold), for what authoring will create.

Draw the plan's scaffold from authoring's folder structure, keeping `spec-plan.md` at the root of `design/` and showing only the folders this project needs. On a documentation platform the same tree maps to the platform hierarchy (`endpoints.md`), and the `intent` page plays the role of `AGENTS.md`.

## Deciding what to recommend

These are planning heuristics, not authoring rules. Adjust them to what the designer said.

- **Always:** `intent.md`, `tokens.md`, `report/` shells, and `motion/`, shown collapsed as one line: authoring copies the skill's four ready motion pages into it at scaffold.
- **`icons.md`:** recommend when the product uses icons beyond a handful.
- **`layouts.md`:** recommend when there are experience specs or responsive behavior.
- **`experience/`:** include when screens or flows are in scope. If more than one experience may share the design system later, say so; authoring decides how to structure it.
- **Scene components:** include one illustrative `scene.<name>.md` only when the brief includes bespoke 3D, 2D or video content.
- **Component library** (`third-party`, `internal` or `mixed`): show one or two illustrative library-backed ui specs, each marked `(library-backed)` in the tree, and mark `tokens.md` with `(library token names)`. In `mixed`, custom components appear as usual.
- **Generative UI:** only when the generative UI answer is yes. A yes adds AI-ready components (Generative use) and a host with Generated content, shown in the tree as inline comments after their names (for example `feature.chat.bundle.md   (generated content)` and `ui.bar-chart.md   (AI-ready)`). A first slice includes the AI-ready components the host allows.
- **`AGENTS.md`:** local markdown endpoint only.

## Showing filenames

Notes about a file or folder go in the tree as inline comments after the name (for example `tokens.md   (library token names)`), never as extra lines.

Show a few **illustrative** filenames drawn from the brain dump (for example `screen.dashboard.md`) and mark them as examples. Authoring builds the real list after scoping. Don't invent a long list.

**Show only first-slice items,** plus the recommended foundation files (`tokens.md`, and `icons.md` and `layouts.md` when recommended), which always appear in the tree. Don't list a file for something you've deferred (for example a deferred map as `scene.map.md`); authoring may create it. Name deferred items in the **Deferred** line instead.

## Scoping to a time-box

When the team has a fixed time (for example three weeks), recommend a thin first slice rather than the whole set. Name it by files or spec types, for example: `intent.md`, the foundation files, the ui and feature components the key screen depends on, and one screen and one flow. It always includes the foundations its screens depend on. State what is *deferred*. Don't produce a schedule; the plan only shapes scope.

## What the scaffold never contains

No values, no component inventory, no status tracking. Specs start as drafts when authoring creates them.
