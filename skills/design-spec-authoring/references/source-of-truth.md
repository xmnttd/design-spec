# Source of Truth

Shared rules for the design-spec family. Read this before writing, previewing, auditing or planning any spec. It defines who owns each kind of fact, so that nothing is stated in two places.

---

## Contents

- The rule
- Ownership map
- Token names in library-backed sets
- `figma` mode
- `spec-only` mode
- Styling values vs. behavioral constants
- Migration from `spec-only` to `figma`
- When a source can't be read

---

## The rule

**Every fact has exactly one owner.** A spec never restates a fact another source owns; it points to it. Duplication is what causes drift: when the same value lives in two places, one of them goes stale, and nobody can tell which.

The project's `visual-source` setting, recorded in `intent.md` only, decides who owns the visual layer:

- `figma`: Figma owns visuals. The default when the project has a Figma file.
- `spec-only`: no Figma. The spec set owns visuals.

Everything else has the same owner in both modes.

---

## Ownership map

| Fact | `figma` mode | `spec-only` mode |
|---|---|---|
| Behavior, intent, logic, data, roles, analytics, content rules | Spec | Spec |
| Accessibility rules (roles, names, keyboard, focus order) | Spec | Spec |
| State and variant names, triggers and behavior | Spec (named exactly as Figma names them) | Spec |
| Visual styling values (color, type, spacing, radius, shadow) | Figma | `tokens.md` (values), `## Visual` sections (references) |
| Layout values (grid, gutters, margins, breakpoint widths, containers) | Figma | `layouts.md` `## Visual` section (`spec-only-mode.md`, §1) |
| Layout rules (reflow, page shells, scroll, sticky, layering order) | `layouts.md` | `layouts.md` |
| Token names | Figma variable path (canonical); `tokens.md` documents the CSS transform. Motion token names are canonical in `motion.tokens`, even when mirrored in Figma. Library-backed sets: "Token names in library-backed sets" below | `tokens.md`; motion token names in `motion.tokens` |
| Icon artwork | Figma | Named installable library or asset file pointers |
| Other assets (logos, illustrations, imagery, font files) | Figma or asset pointers (`assets`; which specs carry it: `spec-types.md`, §6) | Asset pointers (same) |
| Icon rules, sizes, assignments, approved usage | `icons.md` | `icons.md` |
| Scene content (3D, 2D, video, interactive animation) | Asset source (spec holds pointers) | Asset source (spec holds pointers) |
| Scene chrome (controls, labels, legends) and the scene's placement in the layout | Figma | Scene spec `## Visual` section |
| Scene poster image (static fallback frame) | Asset source (spec holds a pointer) | Asset source (spec holds a pointer) |
| Motion (durations, easings, distances, scales, blur, shared delays, defaults, reduced motion) | `motion/` | `motion/` |
| Generative UI: where generated content goes, which AI-ready components are allowed, how many, composition, states around generation, fallbacks, worked examples | The host's `## Generated content` section | Same |
| Generative UI: a component's data contract and rules for the model | The component's Generative use block | Same |
| Library component anatomy, built-in states and interaction, keyboard and ARIA behavior (library-backed sets) | The library's docs at the version in `intent.md`; specs reference them, never restate them | Same |
| Library token architecture (names and roles) | The library's names, documented in `tokens.md` | Same |
| Which library components, variants and options the product uses, and every deviation | Library-backed ui spec | Library-backed ui spec |
| What an AI model generates at runtime | Never a record | Never a record |
| Assistant voice (persona, tone, length, formatting, uncertainty, out-of-scope replies) | `intent.md` | `intent.md` |
| "Ready for dev" | Figma | Not applicable |
| Spec status | Spec front matter | Spec front matter |
| Spec plan (scope, structure, approach before authoring) | `spec-plan.md`, written by `design-spec-plan`; not a spec | Same |
| Code component mapping (which code component implements a spec) | Code Connect or the codebase; never the spec | The codebase; never the spec |
| Decision rationale | `report/decision-log.md` | `report/decision-log.md` |
| Audit findings | `report/audit-register.md` | `report/audit-register.md` |
| Figma annotations | Not a source (`figma-hygiene.md`) | Not applicable |

Behavioral and configuration constants that nothing else owns (performance budgets, camera defaults, zoom limits, debounce and timeout durations, minimum target sizes) belong in the spec in both modes. See "Styling values vs. behavioral constants" below.

**Component libraries.** When `components.source` in `intent.md` isn't `custom`, the library is a third owner next to Figma and the specs. It owns what its docs define at the pinned version; the specs own which parts the product uses and every deviation. Specs never restate the library's docs; they link to them at the pinned version. This paragraph is the one owner of that rule. A library update doesn't change the specs, but deviations should be re-checked against the new version's docs. In `figma` mode, library components come from the library's Figma kit; Figma owns only what the product adds or overrides. In `spec-only` mode, the library's default look with the project's theme is the visual (`spec-only-mode.md`, §1).

**Generative UI.** The model owns nothing. Specs own the rules and the experience; prompts, model choice, safety filters, tool definitions and evaluation tooling belong to engineering and are built from the specs. A host's Generated content criteria and worked examples, and each component's Generative use worked examples, are written so engineering can reuse them as evaluation cases.

**Motion is the exception in both modes.** `motion/` holds motion values (in `motion.tokens`) whether the visual source is `figma` or `spec-only`, and nowhere else holds them. "No visual styling values in specs" (`figma` mode) and "styling values only in `tokens.md`" (`spec-only` mode) don't apply to `motion.tokens`. Figma MAY mirror motion tokens as variables; `motion.tokens` wins where they differ.

---

## Token names in library-backed sets

When the library's names, the Figma variable path and the set's existing names disagree, this order decides which is canonical:

1. **The set's existing names,** if it had tokens before adopting the library. The library's variables are mapped onto them in the Library theme section of `tokens.md`.
2. **Otherwise, the library's names** for the tokens the library defines. In `figma` mode, `tokens.md` records the Figma variable path for each as part of the transform.
3. **Tokens the product adds** follow the naming pattern (`best-practices.md`, §4): the Figma variable path is canonical in `figma` mode, `tokens.md` in `spec-only` mode.

Motion token names are always canonical in `motion.tokens`.

---

## `figma` mode

Figma and the spec set are equals in authority, each within its own space. Figma owns how things look; the spec owns everything else, and tells readers where in Figma to look.

### Pointers

A spec with a visual layer records where that layer lives in Figma:

- **`figma-nodes`:** the file URL (which includes the file key) and the node ID(s) of the main component, component set, section or frame (screens). Never instance IDs, which break when components are recreated or copied. Subfields are defined in `spec-types.md`, §6.
- **`figma-version`:** the Figma file version the spec was verified against, or `lastModified` when the access method doesn't expose a version. It exists so preview can flag a spec that may be out of date and the audit can scope its compare. It is optional and never blocks; when it can't be read: `connections.md`, "File version unreadable". Building doesn't use it.

Which spec types carry pointers:

| Type | Pointers |
|---|---|
| ui-component, feature-component, scene-component, screen | `figma-nodes`, `figma-version` |
| flow | None. Flows own no visuals; the screens they pass through carry the pointers. A prototype link may appear in the flow header for human reference only |
| foundation | Where applicable: `figma-nodes` with `collections` (variable collections, `tokens.md`) or `nodes` (icon library frame for `icons.md`; grid styles and layout frames for `layouts.md`), plus `figma-version` |
| intent | Figma file(s) and preferred read method, in the `figma-access` front matter field |

`figma-version` is updated only after the spec has been verified against the current Figma. Editing a spec without re-reading Figma MUST NOT change it; otherwise drift is hidden. This is the one owner of that rule.

### Ready for dev and buildable

Figma's own "Ready for dev" marker signals that a design is stable. Anything not marked is treated as in flux. The marker is read from Figma and never copied into a spec. This section owns when a spec is buildable, in both modes.

- A component or screen spec is **buildable** when its `status` is `ready` **and** Figma marks it Ready for dev.
- A **library-backed** component: when it has a local override component, that node must be marked Ready for dev too; with no override, its `status` alone decides (the kit is the library's).
- A flow is buildable when its `status` is `ready`. It has no Figma nodes of its own.
- In `spec-only` mode, every spec is buildable when its `status` is `ready`.
- A mismatch in either direction (spec `ready` but Figma not marked, or the reverse) is a finding for the audit.

### What a spec MUST NOT contain in `figma` mode

- Visual styling values: hex codes, pixel sizes for spacing or type, radii, shadows.
- Layout values that Figma owns.
- Icon artwork or inline SVG.
- A copy of "Ready for dev" or any other fact Figma owns.
- A `## Visual` section. A spec with both `figma-nodes` and `## Visual` has duplicated ownership.

### When Figma is missing something

If a state or variant the behavior requires is absent in Figma, the spec still names it, because the state is behavior. The spec does not invent how it looks. The missing variant is a finding for the audit or a blocking question for the designer, depending on urgency.

---

## `spec-only` mode

The spec set owns the visual layer, kept apart from behavior so it can later move to Figma cleanly.

- **Visual styling values live only in `tokens.md`.** Every other spec references token names. A one-off value becomes a new token, never a literal.
- **Layout values and `## Visual` sections:** where they live, which specs carry a `## Visual` section and the library-backed exemption are owned by `spec-only-mode.md`, §1.
- **Assets are pointers**, never inline content: a named installable library, or files with location, format and version.
- No `figma-nodes` or `figma-version` fields.
- Visual authoring rules are in `spec-only-mode.md`.

A spec is buildable when its `status` is `ready` ("Ready for dev and buildable" above).

---

## Styling values vs. behavioral constants

"No raw values" applies to **visual styling**: color, type, spacing, radius, shadow and layout values. These MUST be a token reference, or live in Figma. In `spec-only` mode the one exception is the layout values in `layouts.md` (`spec-only-mode.md`, §1).

**Behavioral and configuration constants** that no other source owns are allowed in the spec in both modes. Examples:

- Performance budgets (frame rate, payload size, load time)
- Scene camera defaults, field of view, zoom and rotation limits
- Debounce, timeout and delay durations that are behavior rather than animation
- Minimum target sizes and other accessibility thresholds
- Input constraints (maximum length, allowed formats)

Animation durations, easings and distances, and shared appearance delays (tooltip dwell, hover intent, loading delay and minimum hold), are motion and belong to `motion/`. Delays that belong to one component's own logic (debounce, timeout, auto-dismiss) stay in that component's spec.

---

## Migration from `spec-only` to `figma`

A project can move to Figma in one sweep or incrementally.

1. Push tokens and components into Figma (with the designer's explicit approval).
2. In each migrated spec: record `figma-nodes` and `figma-version`, then remove its `## Visual` section. Removing the section is what prevents drift.
3. Strip values from `tokens.md`, leaving names, tier and semantics, once every spec that depended on them has migrated.

There is no migration flag. A spec's state is evident from its content:

| Spec contains | State |
|---|---|
| `figma-nodes`, no `## Visual` | Migrated |
| `## Visual`, no `figma-nodes` | Not yet migrated |
| Both | Violation: duplicated ownership |

`visual-source` in `intent.md` is the target mode. It flips to `figma` when the first migration begins. While migration is mid-way, unmigrated specs keep their `## Visual` sections and count as pending migration, not violations.

---

## When a source can't be read

"Couldn't check" is never "checked": never fill a gap from memory, and never switch modes because Figma is unreachable. What to do when Figma, an asset location or the endpoint can't be reached is in `connections.md`, "When a connection is unavailable".
