---
title: scene.hero-animation
type: scene-component
status: in-review
last-updated: 2026-10-02
figma-nodes:
  file: "https://www.figma.com/design/EXAMPLEKEY/Stockroom"
  nodes: ["102:77"]
figma-version: "2214508810"
assets:
  - name: hero-stockroom
    format: riv
    location: "assets/animations/hero-stockroom.riv"
    version: "1.4"
  - name: hero-stockroom-poster
    format: webp
    location: "assets/animations/hero-stockroom-poster.webp"
    version: "1.4"
---

**Role:** An interactive illustration on the welcome screen showing a parcel moving from a shelf to a desk, to explain what Stockroom does at a glance.

**Boundaries:** The scene owns the animation and its interactions. The heading, body copy and "Start a request" action around it belong to the screen. The Figma node covers the play/pause and "Deliver parcel" controls and the scene's placement in the layout; the poster is an asset.

---

## Assets

| Asset | Format | Units and scale | Coordinate convention | Notes |
|---|---|---|---|---|
| `hero-stockroom` | Rive file with one state machine, `Hero` | Artboard 16:9; scales to fit its container | Origin top-left | Contains all animation states |
| `hero-stockroom-poster` | WebP | 16:9 | — | Static first frame; used while loading, on error, on unsupported devices and under reduced motion |

---

## Scene setup

- **Art direction:** flat illustration, gentle and unhurried; the parcel's path reads left to right.
- **Framing:** the full artboard is visible at every size; no cropping.

---

## Interaction model

| Input | Action | Limits | Keyboard alternative |
|---|---|---|---|
| Pointer hover over the parcel | The parcel lifts off the shelf | Pointer devices only | Focus the "Deliver parcel" button |
| Click or tap the parcel | The parcel completes its journey and the desk lamp turns on | Once per playthrough | The "Deliver parcel" button (native `button`) |
| Play/pause control | Pauses or resumes the loop | — | Native button |

**State machine `Hero`:** inputs `hover` (boolean), `deliver` (trigger), `paused` (boolean); states `idle`, `hovering`, `delivering`, `delivered`.

---

## Data contract

- **Inputs:** `paused` (boolean) from the play/pause control; `deliver` (trigger) from the "Deliver parcel" button.
- **Events emitted:** `ready` when the file has loaded; `delivered` when the delivery sequence ends; `error` with a reason code if the file fails to load.
- **Events consumed:** none.

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| loading | Scene enters the viewport | Poster frame shown | None — the poster is static |
| ready | `ready` emitted | Idle loop plays | Default: In-place content change |
| interacting | Hover, click, or the "Deliver parcel" button | State machine responds as above | None — the asset owns this animation |
| paused | Play/pause control, or the scene leaves the viewport | Rendering stops on the current frame | None — the asset owns this animation |
| error | File fails to load | Poster frame remains; no error message is shown, because the poster and its text alternative carry the same content | None — the poster remains |
| unsupported | The device can't run the animation runtime | Poster frame only; play/pause and "Deliver parcel" controls hidden | None — the poster remains |

---

## Performance budgets

| Budget | Target |
|---|---|
| Frame rate | 60 fps on desktop; at least 30 fps on target mobile devices |
| Payload size | Rive file ≤ 150 KB; poster ≤ 60 KB |
| Time to ready | ≤ 1.5 s on a 4G connection |
| Target device tiers | Current and previous-generation desktop browsers; mid-range phones from the last three years |

The scene MUST NOT load until it is near the viewport, and MUST NOT delay the screen's primary content.

---

## Responsive behavior

- Keeps a 16:9 aspect ratio and scales to its container width.
- Hover interactions run on pointer devices only; on touch, tap still delivers the parcel.

---

## Accessibility

- **Text alternative:** "Illustration: a parcel travels from a stockroom shelf to a desk."
- **Role:** the scene is `role="img"` with that text alternative as its accessible name. It is not focusable; its controls are native buttons ("Pause animation" / "Play animation", "Deliver parcel").
- **Moving content:** the loop is paused automatically after 5 seconds of play and can be paused or resumed at any time with the play/pause control.
- **Reduced motion:** when `prefers-reduced-motion` is set, the poster frame is shown and the animation does not autoplay; the "Deliver parcel" button plays the delivery sequence once.

---

## Acceptance criteria

The scene in isolation, including performance, fallback and error cases.

- **AC-1:** **Given** the scene is in the viewport, **when** the file loads, **then** the idle loop MUST play and `ready` MUST be emitted.
- **AC-2:** **Given** keyboard focus is on the "Deliver parcel" button, **when** the user presses `Enter`, **then** the delivery sequence MUST play and `delivered` MUST be emitted.
- **AC-3:** **Given** `prefers-reduced-motion` is set, **when** the scene loads, **then** the poster frame MUST be shown and the animation MUST NOT autoplay.
- **AC-4:** **Given** the file fails to load, **when** the scene renders, **then** the poster frame MUST remain and an error message MUST NOT be shown.
- **AC-5:** **Given** a target mobile device, **when** the idle loop plays, **then** the frame rate MUST stay at or above 30 fps.
- **AC-6:** **Given** the loop has played for 5 seconds, **when** no one has interacted, **then** it MUST pause.
