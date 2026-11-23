---
title: "flow.[name]"
type: flow
status: draft
last-updated: YYYY-MM-DD
---

<!--
STUB — FLOW   (filename: flow.[name].md, verb-led)
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [if applicable] keep only when relevant

A flow is tables, not prose: one action per step, every step names its next,
every non-happy path is a numbered branch.
Flows are behavior only: no Figma fields in front matter and no ## Visual
section. The screens a flow passes through carry their own Figma pointers.
Steps: happy path first. Leave Screen empty for system-only steps.
Acceptance criteria cover the journey across screens only; what happens within
a screen belongs to screen and component specs.
No Motion column: moving between steps follows Route change, or Route change —
top level between top-level destinations (motion.defaults). A step that moves
differently names its default in its System response.
-->

- **Purpose:** [One sentence: the outcome the actor wants.]
- **Actor:** [who]
- **Trigger:** [what starts the flow]
- **Preconditions:** [what must be true before step 1]
- **Prototype:** [if applicable] [link to a prototype, for human reference only; not a source of truth]

---

## Steps

| # | Actor | Screen | Action | System response | Next |
|---|---|---|---|---|---|
| 1 | [User] | `screen.[name]` | [ ] | [ ] | 2 |
| 2 | [User] | `screen.[name]` | [ ] | [ ] | 3 |
| 3 | [System] | | [ ] | [ ] | end |

---

## Branches

| Branch | From step | Condition | Outcome | Next |
|---|---|---|---|---|
| 2a | 2 | [ ] | [ ] | [step number or end] |
| 3a | 3 | [ ] | [ ] | [ ] |

---

## Postconditions

- **Success:** [the state of the system when the flow ends at the happy path.]
- **[Branch outcome]:** [state when it ends at a branch.]

---

## Acceptance criteria

The journey only. Screen and component criteria apply and are not restated.

- **AC-1 (steps 1–3):** **Given** [preconditions], **when** [the actor completes steps 1–3], **then** [postcondition MUST hold].
- **AC-2 (branch 2a):** **Given** [ ], **when** [ ], **then** [the actor MUST return to step 2 with …].
- **AC-3 (state carried):** **Given** [data entered at step 1], **when** [the actor reaches step 3], **then** [it MUST be preserved].
