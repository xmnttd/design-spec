---
title: "feature.[name]"
type: feature-component
status: draft
last-updated: YYYY-MM-DD
parts: ["feature.[name].[part-a]", "feature.[name].[part-b]"]   # [bundle]
figma-nodes:                      # [figma]
  file: "[Figma file URL]"
  nodes: ["[main component or component set node ID]"]
figma-version: "[version or lastModified]"   # [figma]
assets:                           # [if applicable]
  - name: "[asset name]"
    format: "[e.g. svg, png, webp]"
    location: "[repository path or URL]"
    version: "[version]"
---

<!--
STUB — FEATURE COMPONENT   (filename: feature.[name].md · bundle: feature.[name].bundle.md)
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [bundle] keep only for bundles
       [genui] keep only with generative UI: the Generative use block if the model may use
               this component (or one of its parts); the Generated content section, the
               generation state rows and criteria if the model builds inside it (a host)
       [if applicable] keep only when relevant

Prescriptive and product-specific: real copy, a fixed data shape, used in a
specific flow or set of screens.
Front matter: `assets` lists files this component depends on (an
illustration, imagery), as pointers only.
In figma mode, state and variant names match Figma exactly.
Acceptance criteria cover this component in isolation, given its inputs.
Where it appears and how users navigate belong to screen and flow specs.
-->

**Role:** [One sentence: the job this component does, and for whom.]

**Builds on:** [`ui.[name]` | none]. [What this feature adds to it.]

---

## Parts

[bundle]

<!-- Parts exist only inside this feature (for example a navigation bar's buttons). They are named with the bundle's base name plus the part, matching Figma (feature/[name]/[part]). Each part adds only what differs from the parent. Generic building blocks are not parts; they keep their own ui specs. If a part is ever used elsewhere, promote it to its own spec. -->

- **`feature.[name].[part-a]`:** [what it contains and does.]
- **`feature.[name].[part-b]`:** [ ]

---

## Content and copy

| Element | Copy | Notes |
|---|---|---|
| [Heading] | "[Exact text]" | [Max length, truncation, pluralization] |

---

## Data contract

| Field | Type | Required | Source | Notes |
|---|---|---|---|---|
| [field] | [type] | [yes/no] | [API, user input, derived] | [Format, empty value handling] |

---

## States

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | — | [ ] | — |
| loading | [Data requested] | [ ] | Default: Loading |
| empty | [No data] | [Message and next action] | [Default: name / None — reason] |
| error | [Request fails] | [Message and recovery] | [Default: name / None — reason] |
| [genui] generating | [Request sent] | [What shows before anything arrives] | [Default: Loading] |
| [genui] streaming | [Content arriving] | [How text and components appear as they arrive; Stop is available] | [Default: Content entrance] |
| [genui] complete | [Generation finished] | [Final content; regenerate or retry if offered] | [None — the content stays as it is] |
| [genui] partial | [Stopped by the person (where this host offers Stop), or cut off] | [What stays, and how it's marked as incomplete] | [None — reason] |
| [genui] invalid | [Output breaks a rule or a data contract] | [The matching fallback in Generated content] | [Default: In-place content change] |
| [genui] refusal | [Out of scope, or can't help] | [The out-of-scope pattern from the assistant voice] | [Default: Content entrance] |
| [other] | [ ] | [ ] | [Default: name / None — reason] |

<!-- Motion: names from `motion.defaults` only, never values (`motion`, "How specs reference motion"). The default row names motion when the component appears, loops or shows a changing value; runtime-switched variants name theirs in the Variants "Differs by" cell. Add `(moment)` when the change is rare and matters (`Default: Surface enter — modal (moment)`); baseline ignores the mark, a style applied later uses it. [genui] The error row also covers a failed generation request. Generation rows cover only the content around the generated components, which keep their own states and motion from their specs. -->

---

## Variants

[if applicable]

<!-- Deltas only; never restate the default. -->

| Variant | Differs by |
|---|---|
| [e.g. density=compact] | [ ] |

---

## Behavior

- [Rule, using MUST / SHOULD / MAY.]
- [Validation, sort order, input constraints.]
- [if applicable] `Optics: none` where this component must never carry an optical effect.

---

## Roles and permissions

[if applicable]

| Role | Sees | Can do |
|---|---|---|
| [role] | [ ] | [ ] |

---

## Analytics events

[if applicable]

| Event | Fires when | Properties |
|---|---|---|
| [event_name] | [ ] | [ ] |

---

## Error handling

| Error | Cause | User sees | Recovery |
|---|---|---|---|
| [ ] | [ ] | "[Message]" | [ ] |

---

## Content and localization

[if applicable] [Text expansion allowance, date/number/currency formats, right-to-left behavior, strings that must not be translated.]

---

## Generated content

[genui]

<!-- Only for a host: the component or screen where the model builds by placing AI-ready components. This section owns where, what and how many; each allowed component's Generative use block owns its data and rules, so don't restate them. The states around generation are [genui] rows in States; the rules that must always hold are in Acceptance criteria. Engineering builds prompts, tools and evaluations from this; the model owns nothing, and what it generates at runtime is never the record. -->

### Allowed

<!-- Only AI-ready components available as `standalone` or `both`. Components nested inside them come with them and aren't listed. -->

| Component | Where | Notes |
|---|---|---|
| `feature.[name]` | [slot name] slot, or [region] | [e.g. one per answer] |
| `ui.[name]` | [ ] | [ ] |

### Composition rules

- **Always first:** [e.g. a one- or two-sentence text answer.]
- **Limits:** [e.g. at most 2 data components per answer.]
- **Order:** [e.g. suggestions come last.]
- **Pairing:** [e.g. a plan card never appears without a text explanation before it.]
- **Never together:** [ ]

### Content rules

[Length and format on top of the Assistant voice in `intent.md`. Only what differs.]

### Fallbacks

<!-- These override the project-wide default in intent.md (Rules and guardrails, Generative UI) for this host. Where a case isn't listed here, the intent default applies. -->

| When | Renders |
|---|---|
| A component isn't allowed here | [e.g. dropped; the text answer remains] |
| A component's data is missing or invalid | [e.g. the text answer only, plus a short message] |
| A composition limit is exceeded | [e.g. the first n components render, in order; the rest are dropped] |
| The whole output is unusable | [e.g. the error state] |

### Accessibility

- **Announcements:** [generated content is announced once complete, not token by token; how.]
- **Focus:** [stays where it is when generated content arrives; it never jumps.]
- Generated components keep their own accessibility from their specs.

### Worked examples

<!-- At least 3, including one refusal and one fallback. They double as engineering's evaluation cases. -->

| Request | Expected composition | Why |
|---|---|---|
| ["…"] | [message + `ui.[name]` (sample data) + 2 suggestions] | [ ] |
| ["…"] | [ ] | [ ] |
| ["…" (out of scope)] | [refusal message + suggestions] | [ ] |
| ["…" (data missing)] | [fallback: …] | [ ] |

---

## Generative use

[genui]

<!-- Marks this component AI-ready: without this block the model never uses it. One block for the component. [bundle] One block per AI-ready part instead, headed by the part name. Don't name hosts here: each host's Generated content lists what it allows. Nesting: at most two levels, no cycles, and every slot points to an existing spec (`genui.md`, §4). -->

[bundle] ### `feature.[name].[part-a]`

- **Available as:** [`standalone` (a host may place it directly) | `nested` (only inside another AI-ready component) | `both`].
- **When to use:** [one or two sentences written for the model: the question or situation this component answers.]
- **Data contract:** [every field the model fills: name, type, limits, required or optional, and which must come from real data, never invented. For a slot filled by another AI-ready component, reference its contract instead of restating it, e.g. "chart: per `ui.[other]`, variant `[variant]`".]
- **Variants it may pick:** [only the variants the model may choose, e.g. `embedded` inside a card.]
- **When activated:** [what happens when the person interacts with it inside generated content: for example, opens a screen or starts a flow. Actions never run from generated content unless this spec says so.]
- **Never:** [anything the model must not do with it.]
- **Worked examples:** [1–3: sample data → what renders.]

---

## Accessibility

- [Role, accessible names, announcements for dynamic changes (for defaults listed under "Announcements" in `motion`, what is announced; the live region follows `motion` unless stated), keyboard (reference the WAI-ARIA Authoring Practices pattern page where one applies; list deviations only), focus management.]
- [if applicable] **Reduced motion:** [only where this component differs from `motion.reduced`.]

---

## Visual

[spec-only]

<!-- Token references only. Describe only what differs from the ui component it builds on. -->

---

## Acceptance criteria

Behavior of this component in isolation. Screen composition and navigation are not tested here. [genui] Generated content criteria are rules that must always hold, whatever the model generates; the worked examples carry the fixed cases.

- **AC-1:** **Given** [context], **when** [action], **then** [expected result using MUST].
- **AC-2:** **Given** [empty data], **when** [component renders], **then** [ ].
- **AC-3:** **Given** [the request fails], **when** [ ], **then** [ ].
- [genui] **AC-4:** **Given** any generated content, **when** it renders, **then** every component MUST be one listed in Allowed.
- [genui] **AC-5:** **Given** [ ], **when** [ ], **then** [data rule, e.g. figures MUST come from the person's data, never invented].
- [genui] **AC-6:** **Given** the output breaks a rule, **when** it renders, **then** the matching fallback MUST render.
