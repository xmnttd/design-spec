# Generative UI

Authoring only. The one guide for generative UI: the components an AI model may use, the hosts where it builds, and what a host that holds a conversation with an AI assistant should cover. Load it when the product has generative UI or an AI assistant. References and criteria are defined in `spec-types.md`; ownership is in `source-of-truth.md`.

---

## Contents

1. Three things
2. Writing a Generative use block
3. Writing a host's Generated content
4. Nesting
5. Conversational states (AI chat)

---

## 1. Three things

Generative UI is three things on ordinary specs:

1. **Pieces:** ordinary ui and feature components (and scene components, where it fits). Each exists once in Figma and once in the specs.
2. **The "AI can use this" flag:** a **Generative use** block in the component's spec. A component with it is AI-ready; a component without it can never be used by the model.
3. **The host's Generated content:** the component or screen where the model builds (a chat bundle, a dashboard screen) has a `## Generated content` section.

Ownership runs one way each: the host's Generated content owns where, what and how many; each component's Generative use owns its own data and rules. The model owns nothing; what it generates at runtime is never the record.

Only generation that chooses from AI-ready components is covered; composing whole layouts or generating new UI code at runtime is out of scope. Text the model writes inside a fixed component (a plain chat reply) needs neither: the host and the assistant voice in `intent.md` cover it.

Write the Generative use blocks once the component specs exist, and a host's Generated content when writing the host (`work-path.md`, step 7). If the input is loose, read `examples/feature.assistant-panel.md` (a host) and `examples/ui.badge.md` (an AI-ready component) first.

---

## 2. Writing a Generative use block

In ui and feature specs, and optionally scene specs, from the stub's Generative use block (`[genui]`). Screens, flows and foundations never carry one. In a bundle, a part the model may use gets its own block in the bundle's spec, under the part's name.

- **Available as:** `standalone` (a host may place it directly), `nested` (only inside another AI-ready component) or `both`.
- **When to use:** one or two sentences written for the model.
- **Data contract:** strict fields the model fills: name, type, limits, required or optional, and which must come from real data, never invented. For a slot filled by another AI-ready component, reference its contract instead of restating it ("chart: per `ui.bar-chart`, variant `embedded`").
- **Variants it may pick:** only the variants the model may choose (for example `embedded` inside a card).
- **When activated:** what happens if the person interacts with it inside generated content (opens a screen, starts a flow). Actions never run from generated content unless the spec says so.
- **Never:** what the model must not do with it.
- **Worked examples:** 1–3, sample data → what renders.

The block never names hosts. The component behaves exactly as its own spec says: if it needs to behave differently in generated content, that difference goes into its spec as a state or variant.

A composed component made of other AI-ready components (for example `feature.card-chart`: a card header, `ui.bar-chart`, a description and an optional button) is just a normal feature (or ui) component with this block. Its data contract references the nested components' contracts.

Acceptance criteria stay at each spec's own level: feature and scene components carry theirs; ui components verify against their states and Generative use block (`spec-types.md`, §9).

---

## 3. Writing a host's Generated content

In the host's spec (a feature component or a screen), from the stub's `## Generated content` section (`[genui]`). The host keeps its own chrome and states. Don't restate the allowed components' data contracts; they live in each component's Generative use block.

- **Allowed:** which AI-ready components the model may place here, and where (slot or region). Each must be `standalone` or `both`. Components nested inside them come with them and aren't listed.
- **Composition rules:** order, limits, pairing, never-together, and what comes first (for example a text answer).
- **Content rules:** length and format on top of the Assistant voice in `intent.md`. Only what differs.
- **States around generation:** generating, streaming, complete, partial (where the host offers Stop), invalid, refusal and error. They are rows in the host's existing States table, marked `[genui]` in the stub, each with a Motion cell using the existing motion defaults; never a second table.
- **Fallbacks:** a component isn't allowed, its data is missing or invalid, a limit is exceeded, the whole output is unusable. These override intent's Generative UI default for this host.
- **Accessibility:** generated content is announced once complete, and focus stays put. Components keep their own accessibility.
- **Acceptance criteria:** rules that must always hold, whatever the model generates (only allowed components; data never invented; the fallback renders), in the host's Acceptance criteria section.
- **Worked examples:** at least 3, including one refusal and one fallback: request → expected composition. They double as engineering's evaluation cases.

For a host that holds a conversation with an AI assistant, also check the conversational states below.

---

## 4. Nesting

- **At most two levels:** a host places a component, which may hold AI-ready components in its slots; those hold none.
- **No cycles:** a component never nests itself, directly or through another.
- **Every slot points to an existing spec** that is AI-ready and available as `nested` or `both`.

---

## 5. Conversational states (AI chat)

Guidance, not a rule, for components that host a conversation with an AI assistant (a chat panel, an assistant card). Include those that apply:

- **Welcome or empty:** what the assistant can help with, and starter suggestions.
- **Thinking and streaming:** a visible sign the assistant is working, and text arriving as it streams; a way to **stop** it.
- **Complete:** the finished answer, with **regenerate** or **retry** where it makes sense.
- **Edit:** changing a sent message, if supported, and what happens to the answer after it.
- **Error and rate limit:** what happened and what to do next, in the content voice.
- **Refusal or out of scope:** how the assistant says it can't help, and where to go instead.
- **Transparency:** the assistant is labelled as AI; answers based on the person's data or on sources say so; known limits are disclosed.
- **Control and feedback:** feedback on answers (for example thumbs), reporting a problem, and hand-off to a person where the product has one.
- **Privacy:** what the conversation history keeps, and how to clear it.

Generated content inside the conversation is covered by the host's Generated content section and the components' own specs; this guidance covers the conversation around it. Engineering owns how the model is made to behave (prompts, model choice, filters); the specs own what the person experiences.
