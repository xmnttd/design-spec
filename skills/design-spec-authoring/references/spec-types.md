# Spec Types

Shared rules for the design-spec family: the core schema. Defines every spec type, how specs are named, structured and referenced, and the front matter each one carries. Ownership of facts is defined in `source-of-truth.md`; this file assumes it. How authoring recommends a type, detects a bundle and aligns Figma names is in `choosing-types.md` (authoring only).

---

## Contents

1. Types at a glance
2. Type definitions
3. Bundles
4. Folder structure
5. Naming rules
6. Front matter
7. Statuses
8. References between specs
9. Acceptance criteria
10. Required content by type

---

## 1. Types at a glance

| `type:` value | Filename | Folder | What it is |
|---|---|---|---|
| `intent` | `intent.md` | `design/` | Index and project settings: principles, rules, routing, content voice, Figma access |
| `foundation` | `tokens.md`, `icons.md`, `layouts.md`; motion: `motion.md`, `motion.tokens.md`, `motion.defaults.md`, `motion.reduced.md` | `design/design-system/`; motion pages in `design/design-system/motion/` | System-wide foundations |
| `ui-component` | `ui.<name>.md` | `design/design-system/components/` | Generic, non-prescriptive building block |
| `feature-component` | `feature.<name>.md` | `design/design-system/components/` | Prescriptive, product-specific component |
| `scene-component` | `scene.<name>.md` | `design/design-system/components/` | Bespoke 2D/3D, video or interactive-animation component |
| `screen` | `screen.<name>.md` | `design/experience/screens/` | One screen, composed of components and a layout |
| `flow` | `flow.<name>.md` | `design/experience/flows/` | A task path across one or more screens |

`type:` values are hyphenated so they are safe to use in code. Filename prefixes are `ui.`, `feature.`, `scene.`, `screen.` and `flow.`. Foundation files and `intent.md` have fixed names and no prefix; the motion pages share the `motion` name (`motion`, `motion.tokens`, `motion.defaults`, `motion.reduced`).

---

## 2. Type definitions

**ui-component.** Generic building blocks and shells of the UI. Labels and copy are placeholders, there is no fixed data shape, and the component can appear across unrelated screens and flows. Examples: button, text input, badge, generic card, modal shell, side-menu shell.

**Library-backed ui components.** When `components.source` in `intent.md` isn't `custom`, a ui component that comes from the library is still a `ui-component` spec, but a short one: what it's built on, which options this product uses, usage and deviations (`stubs/ui-component.md`, the Library block). The library's docs own the rest. Unmodified library components still get a spec, because which options the product uses is a product decision and screens cite components by spec name. Custom components in a `mixed` set use the full stub.

**feature-component.** Prescriptive: real-world copy and use cases, bound to a specific data shape, appearing inside a specific flow or set of screens. Feature components are also UI, but product-specific. Examples: an order summary panel, an order form, navigation for a specific screen. A feature often builds on a ui component (`feature.side-menu-request` builds on `ui.side-menu.bundle`).

**scene-component.** A component whose content is a special 2D or 3D visual, video or motion-heavy interactive animation authored outside Figma. Still a component of a screen or feature, but bespoke. Examples: an aircraft and cabin visualization in a configurator, a 3D or video hero, an interactive animation built in a tool such as Rive. A generic, reusable viewer or player is a ui-component instead.

**Generative UI.** The components an AI model may use are ordinary ui and feature components (and scene components, where it fits), each existing once in Figma and once in the specs. A **Generative use** block marks a component AI-ready; a component without it is never used by the model. The component or screen where the model builds (a host, such as a chat bundle or a dashboard screen) has a `## Generated content` section. Only generation that chooses from AI-ready components is covered; composing whole layouts or generating new UI code at runtime is out of scope. Text the model writes inside a fixed component (a plain chat reply) needs neither: the host and the assistant voice in `intent.md` cover it. How to write both: `genui.md`.

**screen.** One screen. States and breakpoints live inside the screen spec, never in separate files.

**flow.** A task path: who does what, in what order, across which screens, and what happens when things go wrong.

---

## 3. Bundles

A bundle is one spec that covers a component and its sub-components, when the parts are never used on their own. Example: a modal's header, body and footer may be separate components in Figma, but they only ever appear together, so one spec speaks to all three.

- Bundles apply to ui, feature and scene component specs. A **feature bundle** covers a product-specific component and parts that exist only inside it, for example a navigation bar and its buttons when the buttons are used nowhere else and there is only one navigation bar (`feature.navigation-bar.bundle`, part `feature.navigation-bar.button`). A **scene bundle** does the same for a scene's chrome, for example controls or a legend built as separate Figma components that only ever appear with that scene (`scene.globe.bundle`, part `scene.globe.controls`). Generic building blocks nested in a bundle are never its parts: they keep their own ui specs. Without a bundle, a component describes its internal pieces in its own sections, and its `figma-nodes` may list several nodes.
- Filename postfix `.bundle`: `ui.modal.bundle.md`, `ui.side-menu.bundle.md`, `feature.navigation-bar.bundle.md`, `scene.globe.bundle.md`.
- Front matter lists the parts in `parts:`. Parts are not references to other specs. Parts are named with the bundle's base name plus the part, mirroring Figma: `ui.modal.header` (Figma `ui/modal/header`), `feature.navigation-bar.button` (Figma `feature/navigation-bar/button`). A part keeps the bundle's type prefix. Parts have no files of their own.
- A `## Parts` section describes only what each part adds; it does not repeat the parent.
- Bundle only parts that are never used independently. A nested instance of a generic building block (a badge, button or icon button inside a modal) is never a part. If a part is later used on its own or in another bundle, promote it to its own spec (for example `ui.modal-header`, renamed in Figma to `ui/modal-header`) and update the references.

---

## 4. Folder structure

```
design/
├── AGENTS.md              (local endpoints only)
├── intent.md
├── spec-plan.md           (not a spec; written by design-spec-plan, if used)
├── design-system/
│   ├── components/        (ui., feature. and scene. specs)
│   ├── motion/            (copied from the skill at scaffold, ready to build from)
│   │   ├── motion.md
│   │   ├── motion.tokens.md
│   │   ├── motion.defaults.md
│   │   └── motion.reduced.md
│   ├── icons.md
│   ├── layouts.md
│   └── tokens.md
├── experience/
│   ├── screens/
│   └── flows/
└── report/                (not specs)
    ├── audit-register.md
    └── decision-log.md
```

- Folders stay flat. No category subfolders inside `components/`, `screens/` or `flows/`; they drift the way Figma pages do.
- `report/` holds operational records, not specs. Each file has one skill that writes it: the audit writes the register (authoring scaffolds its empty page, the audit creates it if it's missing, and designers may edit a finding's Status and Note lines); the authoring skill writes the decision log.
- `spec-plan.md` is the plan from `design-spec-plan`, not a spec: no front matter, no status, no spec type. Only `design-spec-plan` writes it; authoring reads it, and preview and audit skip it. The one exception: if `design-spec-plan` saved it locally because it couldn't reach the documentation platform, authoring MAY push it unchanged to the platform once, then discard the local copy.
- `motion/` is copied unchanged from the skill's `motion/` folder at scaffold, ready to build from. Afterward it is edited like any other spec, only when the designer asks, for example to customize the product's motion (`lifecycle.md`).
- Generated output (such as a preview page) is not a spec and lives outside this tree.

On a documentation platform, the same tree maps to the platform hierarchy: folders become folders where the platform supports them, otherwise parent pages with a child-page listing. Page titles are the filename minus `.md`. `endpoints.md` covers the details.

---

## 5. Naming rules

- Lowercase kebab-case within a name; no spaces, no version numbers. Dots only separate the segments: prefix, name, bundle part and `.bundle` (`ui.modal.bundle`, part `ui.modal.header`).
- Lead with the base component when there is one, so related specs sort together: `ui.panel.bundle`, `feature.panel-order-summary`, `feature.panel-key-insights`.
- Screens are noun-led (`screen.dashboard`); flows are verb-led (`flow.add-inventory`).
- Names are unique across the whole project, prefix included.
- **Library-backed names:** in a set that starts with a library, a library-backed spec takes the library component's name in kebab-case (MD3 "Dialog" → `ui.dialog`, "Bottom sheet" → `ui.bottom-sheet`, "Segmented button" → `ui.segmented-button`; shadcn/ui `DropdownMenu` → `ui.dropdown-menu`), so the spec, the Figma kit and the code use one name. Variant and option names follow the library too (MD3 buttons: `filled`, `tonal`, `outlined`, `text`). A set that adopts a library after its specs exist (the adoption is logged in the decision log) keeps its names, since renaming breaks references; "Built on" and the Property map record the mapping, and a rename can still follow `lifecycle.md` ("References") if the designer wants it. A product component built from several library components (a drawer at compact width, a dialog at regular width) keeps a product name and lists each in "Built on".
- States and variants are named in the spec exactly as Figma names them.

### Matching names in Figma (`figma` mode)

The type prefix lives in **both** the Figma section and the component name. Sections organize the canvas, but an instance carries its component's name wherever it is used, so the name is what links a design back to its spec.

Figma uses its native slash convention; specs use dots. The mapping is mechanical:

| Figma | Spec |
|---|---|
| Component `ui/button` | `ui.button` |
| Component set `ui/modal`, parts `ui/modal/header`, `ui/modal/body`, `ui/modal/footer` | `ui.modal.bundle` (parts `ui.modal.header`, `ui.modal.body`, `ui.modal.footer`) |
| Component `feature/panel-order-summary` | `feature.panel-order-summary` |
| Component `feature/navigation-bar`, part `feature/navigation-bar/button` | `feature.navigation-bar.bundle` (part `feature.navigation-bar.button`) |
| Component `scene/globe` | `scene.globe` |
| Frame `screen/dashboard`; state and breakpoint frames `screen/dashboard/<state>/<breakpoint>` (for example `screen/dashboard/empty/compact`) | `screen.dashboard` |
| Prototype flow starting point `flow/submit-request` (naming only; flows carry no Figma pointer) | `flow.submit-request` |

- **Library kit components** (from `components.figma-kit`) keep the kit's names; this alignment applies only to the project's own components. A library-backed spec's `figma-nodes` points at the kit component by `library-key`, plus any local override component in `nodes` (§6).
- Slashes become dots, and the `.bundle` postfix is added in the spec only; Figma's part structure already shows the bundle.
- Slash naming also groups components by type in Figma's Assets panel.
- ui, feature and scene components sit in separate Figma sections (or pages) named `ui`, `feature` and `scene`, in that order.

**Node IDs are the binding; names are alignment.** A spec links to Figma through `figma-nodes`, so it stays correct even if names differ. Matching names exists for humans moving between the two. A name mismatch is a finding, not a blocker. A Figma file with its own conventions is never renamed silently: authoring proposes an alignment map for the designer to approve (`choosing-types.md`), and the audit monitors ongoing drift.

---

## 6. Front matter

Every spec starts with YAML front matter. On a documentation platform it is translated into the platform's native page metadata (see `endpoints.md`), in the same order.

### Field order

Fields always appear in this order. Fields that don't apply are omitted, never left empty.

1. **Identity:** `title`, `type`, `status`, `last-updated`
2. **Motion domain** (`motion` only): `baseline`, `style`, `pages`
3. **Structure:** `parts`
4. **Visual pointers:** `figma-nodes`, `figma-version`
5. **Assets:** `assets`
6. **Project settings** (intent only): `project`, `visual-source`, `endpoint`, `components`, `figma-access`, `audit-register`, `decision-log`, `spec-plan`, `figma-comps`

### All specs

| Field | Value |
|---|---|
| `title` | The spec name (filename minus `.md`); also the page title on documentation platforms |
| `type` | One of the `type:` values above |
| `status` | `draft`, `in-review`, `ready`, `implemented` or `deprecated` |
| `last-updated` | `YYYY-MM-DD` |

### By type

| Field | Applies to | Notes |
|---|---|---|
| `figma-nodes` | ui-component, feature-component, scene-component, screen, and foundation files where applicable (`figma` mode). Never on flows | `file`: the file URL. `nodes`: node IDs of main components, component sets, sections or frames (screens); never instances. `library-key` (library-backed specs only): the published key of the kit component it's built on; `nodes` then holds only a local override component, if any. On `tokens.md` and `motion.tokens.md` (only when motion tokens are mirrored in Figma), `collections` (variable collection names) replaces `nodes`; on `icons.md` and `layouts.md`, `nodes` points at the icon library frame or the grid styles and layout frames |
| `figma-version` | Wherever `figma-nodes` appears, when it can be read | File version, or `lastModified` if no version is available. Optional: omitted when neither can be read. When it's updated: `source-of-truth.md`, "Pointers" |
| `baseline` | `motion` only | The version of the skill's motion pages the project was copied from, quoted (`"<x.y>"`). Changed only by a baseline upgrade (`lifecycle.md`), never by customizing motion |
| `style` | `motion` only | `none`. The predesigned styles `motion` mentions aren't included in this version of the skill; customize the pages instead |
| `pages` | `motion` only | The motion pages that make the motion domain complete: `[motion, motion.tokens, motion.defaults, motion.reduced]` |
| `parts` | ui, feature and scene component bundles only | Part names: the bundle's base name plus the part, e.g. `ui.modal.header` |
| `assets` | tokens, icons, ui-component, feature-component, scene-component, screen | List of pointers: name, format, location, version. Never the content itself. For files the spec depends on: font files (tokens), icon files (icons), logos, illustrations and imagery, and a scene's content and poster |
| `project` | intent only | Project name; recorded only here |
| `visual-source` | intent only | `figma` or `spec-only` |
| `endpoint` | intent only | One of two values: `local`, or `"documentation platform: <name>, <root>"` (quoted, because it contains a colon) |
| `components` | intent only | Where the components come from. `source`: `custom` (designed for this product; nothing else needed), `third-party` (an outside library such as Material 3 or shadcn/ui), `internal` (the organization's own design system, owned by another team) or `mixed` (a library plus custom components). Otherwise also, in this order: `library`, `version` (pinned), `base` where the library offers a choice of underlying primitives or style (shadcn/ui: Base UI, Radix or React Aria), `docs`, and, in `figma` mode, `figma-kit`. `version` is whatever identifies what engineering installs: a package version, or for copy-in libraries the CLI release with the base. If the docs aren't versioned, `docs` adds the date they were read (`https://…, read 2026-10-13`). Missing means `custom` |
| `figma-access` | intent only (`figma` mode) | `files`: one entry per Figma file, each with `url`, `role` (`library`, `composition`, or `library and composition` when one file holds both) and, for library files, `library-key` (the published library key). `read-method`: `MCP`, `REST` or `export` |
| `audit-register`, `decision-log` | intent only | Locations or page IDs of the `report/` pages |
| `spec-plan` | intent only, when a spec plan exists | Location or page ID of `spec-plan.md`, so every skill finds the plan without searching (on a documentation platform it may live under a different parent page) |
| `figma-comps` | intent only, `spec-only` mode | The Figma file of comps generated from the specs (`spec-only-mode.md`, §9). Derived output, never a source: no spec's `figma-nodes` points into it |

### Never in front matter

- "Ready for dev": read from Figma.
- `visual-source` on individual specs: it is project-level, in `intent.md` only.

### Examples

```yaml
---
title: ui.modal.bundle
type: ui-component
status: draft
last-updated: 2026-10-02
parts: [ui.modal.header, ui.modal.body, ui.modal.footer]
figma-nodes:
  file: "https://www.figma.com/design/<file-key>/<file-name>"
  nodes: ["123:456"]
figma-version: "1234567890"
---
```

```yaml
---
title: scene.globe
type: scene-component
status: in-review
last-updated: 2026-10-02
figma-nodes:
  file: "https://www.figma.com/design/<file-key>/<file-name>"
  nodes: ["210:88"]
figma-version: "1234567890"
assets:
  - name: cabin-model
    format: glb
    location: <asset repository path or URL>
    version: "3.2"
---
```

```yaml
---
title: flow.submit-request
type: flow
status: draft
last-updated: 2026-10-02
---
```

---

## 7. Statuses

| Status | Meaning | Who sets it |
|---|---|---|
| `draft` | Being written or changed | Written on new content and on any substantive edit |
| `in-review` | Submitted for team or client review | Designer only |
| `ready` | Safe for engineering to build from | Designer only |
| `implemented` | Engineering or a coding agent has built from this spec; code now depends on it | Designer or engineering only |
| `deprecated` | Retired or superseded by a different spec; kept for reference with a `Replaced by` line directly under its Role line | The authoring skill, when the designer asks to retire the spec |

The usual order is `draft` → `in-review` → `ready` → `implemented`. This section is the one owner of the status-on-edit rule; other files point here.

- **What counts as substantive.** A substantive edit changes behavior, states, acceptance criteria, data contracts or visuals. Reference, pointer and housekeeping edits are not substantive and keep the current status: updating references after a rename, adding or updating `figma-nodes`, `figma-version` or the `spec-plan` field, backfilling a Motion column from the defaults, updating folder descriptions, fixing a typo.
- **Substantive edits to an `in-review`, `ready` or `implemented` spec:** make the edit and set the status to `draft` in the meantime, because the content has changed. At the checkpoint, ask the designer whether it should stay `draft` (more changes pending) or move to `in-review` (needs sign-off) or `ready` (final; needs implementing), then apply their choice. The status question never blocks an edit the designer has asked for.
- **The authoring skill never sets `in-review`, `ready` or `implemented` on its own.**
- **Edits to an `implemented` spec create implementation debt:** code already reflects the old version. Every substantive edit to an `implemented` spec is logged in the decision log (`lifecycle.md`, "Decision log"), so engineering can see what changed after they built it. Once the spec is set to `draft`, that entry is the record that code depends on it.
- **A rename is not a retirement.** Renaming keeps the spec's identity and status; `deprecated` is only for a spec that is retired or replaced by a different spec.
- When a component or screen is buildable: `source-of-truth.md`, "Ready for dev and buildable".

---

## 8. References between specs

Specs refer to each other by **spec name**: the filename minus `.md` (for example `ui.side-menu.bundle`). Where the endpoint supports links, the reference is a link. Each relationship is recorded in one direction only, by the spec that owns it:

- Screens cite the components and the layout they use.
- Flows cite the screens they pass through.
- Feature components cite the ui component they build on, if any.
- A host's Generated content cites the AI-ready components it allows. AI-ready components don't name their hosts.
- A composed AI-ready component cites the AI-ready components nested in its slots, in its Generative use data contract.
- A library-backed spec links to the library's docs at the version in `intent.md`.
- Bundles list their parts in `parts:`. Parts are not separate specs, so they aren't references and aren't checked as such.
- Components do not list where they are used. The using spec holds the reference, so there is one place to update.

A reference to a missing or `deprecated` spec is broken. When a spec is renamed (in place, with no leftover copy) or deprecated, every spec that references it is updated.

---

## 9. Acceptance criteria

Feature, scene, screen and flow specs MUST include acceptance criteria: checkable statements of how to verify the build matches intent. They are what engineering agents and QA check against, and the part a mockup alone can never carry.

- Format and quality: `best-practices.md`, §8.
- Flow criteria cite step and branch numbers from the flow's tables.
- Scene criteria include performance, fallback and error cases.
- A host's Generated content criteria are rules that must always hold, whatever the model generates (only allowed components; data never invented; the fallback renders when a rule breaks), not fixed outcomes. The worked examples carry the fixed cases.
- ui-components don't carry acceptance criteria; they verify against their states and variants.

### Criterion IDs

This section owns the criterion-ID rules; other files point here.

Within a spec, criteria are numbered `AC-1`, `AC-2`, and so on. The **full ID** is the spec name plus the number, for example `feature.panel-order-summary/AC-3`. Because spec names are unique, full IDs are unique across the whole product without any central register.

- **Never renumber.** A new criterion takes the next free number, even if it's written above existing ones.
- **Retire, don't reuse.** A removed criterion's number is skipped, never given to a new criterion, so tests and audit findings that cite it never silently point at something else. Removed IDs are listed on one line at the end of the acceptance criteria section, `Retired: AC-6, AC-9`, so the next free number is visible where it is chosen. This is one of the two history lines a spec may hold (`best-practices.md`, §1).
- **Flow labels:** a flow criterion names what it covers after its ID: the steps or branch, `AC-1 (steps 1–3)`, `AC-2 (branch 2a)`, or a short descriptive label, `AC-3 (state carried)`. The label is not part of the ID.
- **Cross-spec references use the full ID:** "`feature.form-order/AC-2` applies." Within the same spec, `AC-2` alone is enough.
- **Renames carry over:** renaming a spec changes its full IDs. For an `implemented` spec the rename is logged in the decision log, so engineering can update test references.
- The `spec-name/AC-n` pattern is machine-readable, so a product-wide index of criteria can be compiled from the specs at any time (for example by `design-spec-audit` or `design-spec-preview`) rather than maintained by hand.

### Which level owns a criterion

Each criterion lives at the **lowest level that owns the behavior**; higher levels test only what they add. This works like a test pyramid (unit, integration, end-to-end), and it prevents the same check appearing in three places.

| Level | Owns criteria for | Not for |
|---|---|---|
| feature-component, scene-component | Behavior in isolation, given its inputs: validation, states, how it renders its data, its own errors, (scenes) performance and fallback | Where it appears; navigation |
| Host's Generated content (feature or screen) | What may be generated there: allowed components only, composition rules, data never invented, fallbacks and the states around generation | How each AI-ready component behaves (its own spec) |
| screen | Composition: what loads, how regions interact, screen-level empty/loading/error states, permissions, responsive layout | Internals of its components |
| flow | The journey: step order, navigation, state carried between screens, branch outcomes, postconditions | What happens within a single screen |

- **The scope test:** can the criterion be checked using only this spec's scope? If it needs another spec's internals, it belongs in that spec.
- Higher levels reference lower ones instead of restating them: "`feature.form-order` acceptance criteria apply", or a specific one by full ID, "`feature.form-order/AC-2` applies".
- If two levels seem to own the same check, keep it at the lower one.

Example:

> **Given** the request form has an empty required field, **when** the user selects Submit, **then** the form MUST NOT submit, focus MUST move to the first invalid field, and its error message MUST be announced to assistive technology.

---

## 10. Required content by type

The stubs hold the full templates. This is the summary of what each type covers; sections that don't apply are omitted rather than left empty.

| Type | Core sections |
|---|---|
| intent | Summary, principles, rules and guardrails (including default empty/error/loading rules, the reduced-motion rule, the accessibility target and target-size default and, with generative UI, a Generative UI bullet; the reduced-motion behavior itself lives in `motion.reduced`), content voice (with Assistant voice when the product has an AI assistant), structure and routing. Settings and Figma access live in front matter only; on a documentation platform, a "For agents" section (the `AGENTS.md` role) |
| foundation: tokens | Tier, the five categories (`best-practices.md`, §4), canonical names and CSS transform, semantics, font sourcing and licensing, theme/brand modes if any, and (library-backed sets that kept their own names) a Library theme section |
| foundation: icons | Library or source, sizes, color rule, implementation rules, assignments, approved usage |
| foundation: layouts | Breakpoint names, reflow rules, page shells, scroll/sticky/max-width/layering rules, grid per screen type |
| foundation: motion | Supplied complete by the skill (`motion`, `motion.tokens`, `motion.defaults`, `motion.reduced`); not authored, edited only to customize |
| ui-component | Role, usage (when to use, when not, alternatives), anatomy, properties, states (with a Motion column), variants, behavior, accessibility |
| ui-component (library-backed) | Role, the Library block (built on, property map, used in this product, deviations), usage, properties used, states the product adds or changes (with a Motion column), anything that deviates |
| feature-component | Role, the ui component it builds on, content and copy, data contract, states (with a Motion column), behavior, roles and permissions, analytics events, error handling, content/localization rules, accessibility, acceptance criteria |
| scene-component | Role and boundaries, assets, scene setup, configurable parts, interaction model, data contract, states (with a Motion column), performance budgets, responsive behavior, accessibility, acceptance criteria |
| screen | Purpose, layout used, regions and composition (components used, by spec name), data contract, screen-level states (with a Motion column), responsive behavior, roles and permissions, analytics events, error handling, content/localization rules, accessibility, acceptance criteria |
| flow | Header (purpose, actor, trigger, preconditions), steps table, branches table, postconditions, acceptance criteria |

Also, where they apply:

- **Parts:** every bundle (ui, feature, scene) has a `## Parts` section (§3).
- **Generative use:** an AI-ready ui or feature component (or scene component, or a bundle part) has a Generative use block. Screens, flows and foundations never carry one.
- **Generated content:** a host (a feature component or screen where the model builds) has a `## Generated content` section, its states around generation as rows in its States table, and its rules that must always hold in its Acceptance criteria.
- **Behavior:** scene and screen specs MAY add a Behavior section for behavior that has no other home, such as an exception to the motion defaults. A spec's own reduced-motion behavior goes in its Accessibility section.

### Flow tables

**Steps** (happy path first; one action per step; every step names its next):

| # | Actor | Screen | Action | System response | Next |
|---|---|---|---|---|---|
| 1 | User | `screen.request` | Selects New request | Opens the empty request form | 2 |

The screen column is filled when the step happens on a screen; system-only steps leave it empty.

**Branches** (every non-happy path is a numbered branch, never prose):

| Branch | From step | Condition | Outcome | Next |
|---|---|---|---|---|
| 2a | 2 | A required field is empty | Inline error on the field | 2 |

### Visual sections in `spec-only` mode

In `spec-only` mode, ui, feature, scene and screen specs and `layouts.md` also carry a `## Visual` section, except library-backed ui specs with no overrides. The rule is owned by `spec-only-mode.md`, §1.
