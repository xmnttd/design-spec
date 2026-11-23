---
title: "scene.[name]"
type: scene-component
status: draft
last-updated: YYYY-MM-DD
parts: ["scene.[name].[part-a]", "scene.[name].[part-b]"]   # [bundle]
figma-nodes:                        # [figma]
  file: "[Figma file URL]"
  nodes: ["[node ID]"]
figma-version: "[version or lastModified]"   # [figma]
assets:
  - name: "[asset name]"
    format: "[e.g. glb, mp4, riv, lottie]"
    location: "[repository path or URL]"
    version: "[version]"
  - name: "[poster name]"
    format: "[e.g. webp, png]"
    location: "[repository path or URL]"
    version: "[version]"
---

<!--
STUB — SCENE COMPONENT   (filename: scene.[name].md · bundle: scene.[name].bundle.md)
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [bundle] keep only for bundles · [if applicable] keep only when relevant
       [genui] keep only if the model may use this scene (or one of its parts) in generated content

A component whose content is a 2D/3D visual, video or interactive animation
made outside Figma. The spec holds asset POINTERS only, never the content.
Figma owns the chrome (controls, labels, legends) and the scene's placement;
figma-nodes points at those only.
The poster (static fallback frame) is an asset, listed in assets: like the scene.
State requirements and budgets, not the rendering technology.
Numbers here are behavioral constants (budgets, camera, limits), which are allowed.
-->

**Role:** [One sentence: what the scene shows and why.]

**Boundaries:** [What the scene is responsible for, and what the surrounding screen or feature handles.]

---

## Parts

[bundle]

<!-- Parts are pieces of the scene's chrome built as separate Figma components that only ever appear with this scene (for example its controls or legend). They are named with the bundle's base name plus the part, matching Figma (scene/[name]/[part]). Each part adds only what differs from the parent. Generic building blocks are not parts; they keep their own ui specs. Not to be confused with "Configurable parts" below, which are parts of the scene content. -->

- **`scene.[name].[part-a]`:** [what it contains and does.]
- **`scene.[name].[part-b]`:** [ ]

---

## Assets

| Asset | Format | Units and scale | Coordinate convention | Notes |
|---|---|---|---|---|
| [name, as in front matter] | [ ] | [e.g. 1 unit = 1 m] | [e.g. Y-up, origin at …] | [ ] |

---

## Scene setup

- **Default camera:** [position, target, field of view.]
- **Camera bounds:** [limits.]
- **Lighting and environment:** [the lighting or environment setup, as a named setup inside the scene asset or an asset pointer; never values copied out of the asset.]
- **Art direction:** [the intended look, in words.]

---

## Configurable parts

| Part or variant | Options | Driven by |
|---|---|---|
| [e.g. seat layout] | [ ] | [UI control or data field] |

---

## Interaction model

| Input | Action | Limits | Keyboard alternative |
|---|---|---|---|
| [Drag] | [Orbit] | [Rotation bounds] | [Arrow keys] |
| [Pinch / wheel] | [Zoom] | [Min and max] | [+ / -] |
| [Click hotspot] | [Select part] | | [Tab to hotspot, Enter] |

[if applicable] **State machine:** inputs [names, types] and states [names] exposed by the animation.

---

## Data contract

- **Inputs:** [field, type, effect on the scene.]
- **Events emitted:** `ready`, `selection-changed` [payload], `error` [payload], [others.]
- **Events consumed:** [ ]

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| loading | [Asset requested] | [Poster frame and progress] | [None — the poster is static] |
| ready | [Asset loaded] | [ ] | [Default: In-place content change] |
| interacting | [User input] | [ ] | None — the asset owns this animation |
| idle / paused | [No input for n s, offscreen, or user pauses] | [Rendering pauses] | None — the asset owns this animation |
| error | [Asset fails] | [Poster frame and message] | [Default: name / None — reason] |
| unsupported | [Device can't render] | [Fallback] | [Default: name / None — reason] |

<!-- Motion covers changes visible around the scene only; animation inside the scene belongs to its asset. The scene's own reduced-motion behavior stays in Accessibility. -->

---

## Behavior

[if applicable]

<!-- Behavior with no other home, such as an exception to the motion defaults: what it overrides and how it behaves when interrupted. Reduced-motion behavior goes in Accessibility. -->

- [Rule, using MUST / SHOULD / MAY.]

---

## Performance budgets

| Budget | Target |
|---|---|
| Frame rate | [ ] |
| Payload size | [ ] |
| Polygon and texture limits | [ ] |
| Time to ready | [ ] |
| Target device tiers | [ ] |

---

## Responsive behavior

[Aspect ratio, resize rules, and what degrades on small screens or low-power devices.]

---

## Generative use

[genui]

<!-- Marks this component AI-ready: without this block the model never uses it. One block for the component. [bundle] One block per AI-ready part instead, headed by the part name. Don't name hosts here: each host's Generated content lists what it allows. Nesting: at most two levels, no cycles, and every slot points to an existing spec (`genui.md`, §4). -->

[bundle] ### `scene.[name].[part-a]`

- **Available as:** [`standalone` (a host may place it directly) | `nested` (only inside another AI-ready component) | `both`].
- **When to use:** [one or two sentences written for the model: the question or situation this component answers.]
- **Data contract:** [every field the model fills: name, type, limits, required or optional, and which must come from real data, never invented. For a slot filled by another AI-ready component, reference its contract instead of restating it, e.g. "legend: per `ui.[other]`, variant `[variant]`".]
- **Variants it may pick:** [only the variants the model may choose, e.g. `embedded` inside a card.]
- **When activated:** [what happens when the person interacts with it inside generated content: for example, opens a screen or starts a flow. Actions never run from generated content unless this spec says so.]
- **Never:** [anything the model must not do with it.]
- **Worked examples:** [1–3: sample data → what renders.]

---

## Accessibility

- **Text alternative:** [what the scene conveys, for assistive technology.]
- **Dragging:** every drag action MUST have a non-drag alternative.
- **Moving content:** users MUST be able to pause, stop or hide motion lasting more than five seconds.
- **Reduced motion:** [the reduced alternative when `prefers-reduced-motion` is set.]

---

## Visual

[spec-only]

<!-- Token references for the chrome and placement; art direction for the scene in words. -->

---

## Acceptance criteria

The scene in isolation, including performance, fallback and error cases.

- **AC-1:** **Given** [ ], **when** [ ], **then** [ ].
- **AC-2:** **Given** the device can't render the scene, **when** it loads, **then** the poster frame and [fallback] MUST be shown.
- **AC-3:** **Given** [target device tier], **when** [interacting], **then** the frame rate MUST stay at or above [target].
