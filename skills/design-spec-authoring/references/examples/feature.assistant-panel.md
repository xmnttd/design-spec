---
title: feature.assistant-panel
type: feature-component
status: draft
last-updated: 2026-10-29
figma-nodes:
  file: "https://www.figma.com/design/EXAMPLEKEY/Stockroom"
  nodes: ["91:3107"]
figma-version: "2214508810"
---

**Role:** The Stockroom assistant's chat panel, where employees ask about their requests and what Stockroom stocks, and the assistant answers each message with a short text and at most two components.

**Builds on:** `ui.drawer.bundle`. Adds the conversation, the message input and the generated answers.

---

## Content and copy

| Element | Copy | Notes |
|---|---|---|
| Heading | "Stockroom assistant" | With a `ui.badge` reading "AI" beside it |
| Welcome message | "Ask about your requests or what we stock." | Empty conversation only |
| Input placeholder | "Ask a question" | |
| Send action | "Send" | `ui.button`, `intent=primary` |
| Stop action | "Stop" | `ui.button`; replaces Send while an answer is generating or streaming |
| Disclaimer | "Answers use your requests and the catalog. They can be wrong." | Below the input |

---

## Data contract

| Field | Type | Required | Source | Notes |
|---|---|---|---|---|
| `messages[]` | list | yes | Conversation API | Empty list shows the welcome state |
| `messages[].author` | enum: `employee`, `assistant` | yes | Conversation API | |
| `messages[].content` | text, or an answer (Generated content below) | yes | Employee input; the model | An answer is kept as it rendered |
| `requests[]` | list | yes | Request API | The employee's own requests, passed with each message |

---

## States

Rows from generating on are the states around generation.

| State | Trigger | Behavior | Motion |
|---|---|---|---|
| default | Conversation has messages | Messages oldest first; the input is ready | — |
| empty | No messages yet | Welcome message and 3 `feature.assistant-suggestion` starters | Default: Content entrance |
| error | The request for an answer fails | "Sorry, I couldn't answer that. Try again." with "Retry" | Default: Content entrance |
| generating | The employee sends a message | The typing indicator shows in the conversation; Stop replaces Send | Default: Loading |
| streaming | Content arrives | Text appears as it streams; components appear once their data is complete | Default: Content entrance |
| complete | Generation finishes | The answer stays; suggestions become selectable; Send returns | None — the answer stays as it is |
| partial | The employee selects "Stop", or the answer is cut off | What arrived stays, followed by "Answer stopped" | None — the status line appears with the answer |
| invalid | A component breaks a rule or its data contract | The matching fallback below | Default: In-place content change |
| refusal | The question is out of scope | The out-of-scope reply from the assistant voice, plus 2 suggestions | Default: Content entrance |

---

## Behavior

- Sending a message scrolls the conversation to it. While an answer streams, the conversation doesn't scroll if the employee has scrolled up.
- Selecting a suggestion sends its text as the employee's next message.
- Earlier answers stay in the conversation as they rendered; they're never regenerated in place.

---

## Error handling

| Error | Cause | User sees | Recovery |
|---|---|---|---|
| Answer failed | Model or Conversation API error or timeout | "Sorry, I couldn't answer that. Try again." | "Retry" sends the same message again |

---

## Generated content

### Allowed

| Component | Where | Notes |
|---|---|---|
| `feature.assistant-message` | `messages` slot | The text answer |
| `ui.badge` | `messages` slot | Data component |
| `feature.panel-order-summary` | `messages` slot | Data component |
| `feature.catalog-item-card` | `messages` slot | Data component |
| `feature.assistant-suggestion` | `messages` slot | Follow-up |

One answer per message the employee sends.

### Composition rules

- **Always first:** a `feature.assistant-message` of one or two sentences that answers the question on its own.
- **Limits:** at most 2 data components per answer; at most 3 suggestions.
- **Order:** text, then data components, then suggestions.
- **Pairing:** a `feature.catalog-item-card` always follows text that says why it's shown.
- **Never together:** a `feature.panel-order-summary` and a `feature.catalog-item-card` in one answer.

### Content rules

Text answers follow the assistant voice in `intent`. Here, the text never repeats what a component below it shows; it explains it.

### Fallbacks

| When | Renders |
|---|---|
| A component isn't allowed here | It's dropped; the text answer and other valid components remain |
| A component's data is missing or invalid | The text answer only, plus "I couldn't load those details" |
| A composition limit is exceeded | The first 2 data components and the first 3 suggestions render, in order; the rest are dropped |
| The whole output is unusable | The error state |

### Accessibility

- **Announcements:** the answer is announced once complete, as one update ("Assistant replied"), not as it streams.
- **Focus:** stays in the message input after an answer arrives.
- Generated components keep their own accessibility from their specs.

### Worked examples

| Request | Expected composition | Why |
|---|---|---|
| "Where's my laptop request?" | message + `ui.badge` (`label="Awaiting approval"`, `status=info`) + suggestion "Who approves it?" | One request's status |
| "What's in my last request?" | message + `feature.panel-order-summary` | The request's contents |
| "Do we have USB-C docks?" | message + `feature.catalog-item-card` + suggestion "Add it to a request" | Adding happens in `flow.submit-request`, not in the chat |
| "What's the weather?" | refusal message + 2 suggestions | Out of scope |
| "Show my requests from last year" (no data before this year) | fallback: message + "I couldn't load those details" | Data missing for the period |

---

## Accessibility

- The panel follows `ui.drawer.bundle` for its landmark, keyboard and focus behavior, labelled "Stockroom assistant".
- The message input's accessible name is "Ask a question".
- Each message names its author to assistive technology ("You", "Assistant").

---

## Acceptance criteria

Behavior of this component in isolation. Generated content criteria are rules that must always hold, whatever the model generates; the worked examples carry the fixed cases.

- **AC-1:** **Given** an empty conversation, **when** the panel opens, **then** the welcome message and 3 starter suggestions MUST show.
- **AC-2:** **Given** an answer is streaming, **when** the employee selects "Stop", **then** what arrived MUST stay, followed by "Answer stopped", and Send MUST return.
- **AC-3:** **Given** any answer, **when** it renders, **then** every component MUST be one listed in Allowed, and the first MUST be `feature.assistant-message`.
- **AC-4:** **Given** an answer with a `feature.panel-order-summary`, **when** it renders, **then** every item, quantity and price MUST match the request's data.
- **AC-5:** **Given** an answer with more than 2 data components or more than 3 suggestions, **when** it renders, **then** the "limit exceeded" fallback MUST render: only the first 2 data components and the first 3 suggestions show.
- **AC-6:** **Given** a component's data is missing, **when** the answer renders, **then** the text answer and the "couldn't load" message MUST show, and the component MUST NOT.
- **AC-7:** **Given** an out-of-scope question, **when** the answer renders, **then** it MUST use the out-of-scope reply and MUST NOT include data components.
