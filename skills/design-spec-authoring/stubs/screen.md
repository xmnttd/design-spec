---
title: "screen.[name]"
type: screen
status: draft
last-updated: YYYY-MM-DD
figma-nodes:                      # [figma]
  file: "[Figma file URL]"
  nodes: ["[frame node ID]"]
figma-version: "[version or lastModified]"   # [figma]
assets:                           # [if applicable]
  - name: "[asset name]"
    format: "[e.g. webp, svg]"
    location: "[repository path or URL]"
    version: "[version]"
---

<!--
STUB — SCREEN   (filename: screen.[name].md, noun-led)
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [genui] keep only if the model builds on this screen (a host of generated content)
       [if applicable] keep only when relevant

One spec per screen. States and breakpoints live here, not in separate files.
Front matter: figma-nodes lists the screen frame and its state and breakpoint frames where they exist.
`assets` lists imagery this screen depends on (a hero image), as pointers only.
Reference components and the layout by spec name; never restate their internals.
Acceptance criteria cover composition only; component behavior is tested in
component specs, and the journey across screens in flow specs.
-->

**Purpose:** [One sentence: what the user comes here to do.]

**Layout:** [page shell from `layouts.md`, e.g. sidebar-content.]

---

## Regions and composition

| Region | Components | Notes |
|---|---|---|
| [e.g. side menu] | [e.g. `feature.side-menu-request`] | [ ] |
| [e.g. main] | [e.g. `feature.form-order`] | [ ] |

---

## Data contract

| Data | Source | Loaded when | Notes |
|---|---|---|---|
| [ ] | [API] | [On entry, on demand] | [ ] |

---

## States

Screen-level states only; component states live in component specs.

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| loading | [Entry] | [What shows while data loads] | [Default: Route change / Default: Route change — top level] |
| empty | [No data] | [Screen-level message and next action] | [Default: name / None — reason] |
| error | [Load fails] | [Message and recovery] | [Default: name / None — reason] |
| [genui] generating | [Request sent] | [What shows before anything arrives] | [Default: Loading] |
| [genui] streaming | [Content arriving] | [How text and components appear as they arrive; Stop is available] | [Default: Content entrance] |
| [genui] complete | [Generation finished] | [Final content; regenerate or retry if offered] | [None — the content stays as it is] |
| [genui] partial | [Stopped by the person (where this host offers Stop), or cut off] | [What stays, and how it's marked as incomplete] | [None — reason] |
| [genui] invalid | [Output breaks a rule or a data contract] | [The matching fallback in Generated content] | [Default: In-place content change] |
| [genui] refusal | [Out of scope, or can't help] | [The out-of-scope pattern from the assistant voice] | [Default: Content entrance] |
| [ ] | [ ] | [ ] | [Default: name / None — reason] |

<!-- Motion covers screen-level states only. Arriving on the screen follows the matching Route change default (Route change, or Route change — top level between top-level destinations) and isn't repeated. A state reached only by arriving names that default, never None. Add `(moment)` when the change is rare and matters (`Default: Content entrance (moment)` on onboarding); baseline ignores the mark, a style applied later uses it. [genui] The error row also covers a failed generation request. Generation rows cover only the content around the generated components, which keep their own states and motion from their specs. -->

---

## Behavior

[if applicable]

<!-- Behavior with no other home, such as an exception to the motion defaults: what it overrides and how it behaves when interrupted. Reduced-motion behavior goes in Accessibility. -->

- [Rule, using MUST / SHOULD / MAY.]

---

## Responsive behavior

[How regions reflow per breakpoint, beyond what `layouts.md` already defines for this shell. Reference, don't repeat.]

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
| [screen_viewed] | [ ] | [ ] |

---

## Error handling

| Error | Cause | User sees | Recovery |
|---|---|---|---|
| [ ] | [ ] | "[Message]" | [ ] |

---

## Content and localization

[if applicable] [Screen-level copy and formats.]

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

## Accessibility

- **Page title:** [the document title; unique and descriptive.]
- **Focus on arrival:** [where focus goes when the screen opens.]
- **Focus not obscured:** [sticky or overlaid regions never fully hide the focused element.]
- [if applicable] **Announcements:** [for states that use a default listed under "Announcements" in `motion` (Confirmation, Validation error, Pop-in, a toast, Loading, an In-place content change the person didn't cause, a keyboard Reposition): what is announced, with placeholder text. The live region follows `motion` unless stated.]
- [if applicable] **Reduced motion:** [only where this screen differs from `motion.reduced`.]

---

## Visual

[spec-only]

<!-- Composition and spacing between regions only, as token references; everything else comes from the cited components and layout. -->

---

## Acceptance criteria

Composition only. Component acceptance criteria apply and are not restated: [`feature.[name]`, …]. [genui] Generated content criteria are rules that must always hold, whatever the model generates; the worked examples carry the fixed cases.

- **AC-1:** **Given** [ ], **when** the screen loads, **then** [regions and data shown MUST …].
- **AC-2:** **Given** [no data], **when** the screen loads, **then** [screen-level empty state].
- **AC-3:** **Given** [role without permission], **when** [ ], **then** [ ].
- [genui] **AC-4:** **Given** any generated content, **when** it renders, **then** every component MUST be one listed in Allowed.
- [genui] **AC-5:** **Given** [ ], **when** [ ], **then** [data rule, e.g. figures MUST come from the person's data, never invented].
- [genui] **AC-6:** **Given** the output breaks a rule, **when** it renders, **then** the matching fallback MUST render.
