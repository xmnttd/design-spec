---
title: intent
type: intent
status: draft
last-updated: YYYY-MM-DD
project: "[Project Name]"
visual-source: "[figma | spec-only]"
endpoint: "[local | documentation platform: [name], [root]]"
components:
  source: "[custom | third-party | internal | mixed]"
  library: "[library name]"          # [library]
  version: "[pinned version or release]"   # [library]
  base: "[underlying primitives or style]"   # [library] [if applicable]
  docs: "[docs URL]"                 # [library]
  figma-kit: "[Figma library URL]"   # [library] [figma] [if applicable]
figma-access:                     # [figma]
  files:
    - url: "[Figma file URL]"
      role: "[library | composition | library and composition]"
      library-key: "[published library key, if the file is a library]"
  read-method: "[MCP | REST | export]"
audit-register: "[location or page ID of report/audit-register]"
decision-log: "[location or page ID of report/decision-log]"
spec-plan: "[location or page ID of spec-plan]"   # [if applicable]
figma-comps: "[Figma file URL of comps generated from the specs]"   # [spec-only] [if applicable]
---

<!--
STUB — INTENT
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [library] keep only when components.source isn't custom
       [genui] keep only when the product has generative UI
       [assistant] keep only when the product has an AI assistant
       [platform] keep only when the endpoint is a documentation platform
       [if applicable] keep only when relevant

Front matter:
- endpoint is stored exactly as one of the two forms: local, or
  "documentation platform: [name], [root]" (quoted).
- components: with source custom, keep only source. Otherwise library,
  version (pinned; its docs at this version are part of the record),
  base where the library offers a choice of primitives or style (shadcn/ui:
  Base UI, Radix or React Aria), docs and, in figma mode, figma-kit if the
  library has a Figma kit.
- figma-access files: one entry per file; a single file can hold both roles.
- spec-plan: only when a spec plan exists.
- figma-comps: spec-only mode only, when Figma comps are generated from the specs
  (spec-only-mode.md, section 9). Never a source.

intent.md is the index and settings page, read first by every agent.
It holds principles, rules, content voice and routing. It never holds
token values, icon inventories or component details, and it does not
list every spec: folders and child-page listings do that.
-->

[One or two sentences: what this product is, who it serves, and the overall design approach. The project name is in front matter (`project`); don't restate it here.]

Specs use BCP 14 (RFC 2119, RFC 8174) requirement language: MUST, MUST NOT, SHOULD, SHOULD NOT, MAY. Only the uppercase forms are normative.

---

## Principles

- **[Principle]:** [One sentence on what it means in practice.]
- **[Principle]:** [One sentence.]
- **[Principle]:** [One sentence.]

---

## Rules and guardrails

### Defaults every spec inherits

- **Empty states:** [default rule, e.g. components that display dynamic data MUST define an empty state with a message and, where useful, a next action.]
- **Loading states:** [which kind of indicator: skeleton, spinner or progress]. Timing follows `motion.defaults` → Loading.
- **Error states:** [default rule, e.g. errors MUST say what happened and what to do next; inline for fields, banner for page-level.]
- **Motion:** motion is on by default. Every state change follows `design-system/motion/` (`motion.defaults`); when the viewer sets `prefers-reduced-motion`, `motion.reduced` applies. [if applicable] The product's motion setting offers Full, Reduced and None (`motion`, "Motion levels"). A spec states only where it differs.
- **Accessibility target:** WCAG 2.2 AA unless stated otherwise here.
- **Target size:** [default, e.g. at least 24×24px (WCAG 2.2 AA); 44×44px for touch]. A spec states only where it differs.
- [genui] **Generative UI:** [Which hosts have Generated content and which components are AI-ready. Only AI-ready components may render; anything else falls back to the default fallback (a plain text message) unless the host's Generated content says otherwise.]

### Do

- [Specific, actionable rule.]
- [Specific, actionable rule.]

### Don't

- [Anti-pattern, with the reason.]
- [Anti-pattern, with the reason.]

---

## Content voice

- **Tone:** [e.g. plain, direct, calm.]
- **Person and address:** [e.g. second person, "you"; first person plural for the product, or none.]
- **Capitalization:** [e.g. sentence case for all UI text.]
- **Terminology:** [preferred terms and terms to avoid.]
- **Numbers, dates and units:** [formats.]

### Assistant voice

[assistant]

- **Persona:** [who the assistant is, in one line; it is always labelled as AI.]
- **Tone:** [e.g. calm, plain, helpful; how it differs from the product's own voice, if at all.]
- **Length:** [default answer length, e.g. one or two sentences before any component.]
- **Formatting:** [what answers may use: lists, bold, links; what they never use.]
- **Uncertainty:** [how it says it isn't sure, and when it says what an answer is based on.]
- **Out of scope:** [the pattern for saying it can't help, and where it points the person instead.]

---

## Structure and routing

| Location | Holds |
|---|---|
| `design-system/tokens.md` | Token tier, names and semantics [spec-only: and values] |
| `design-system/icons.md` | Icon source, sizes, rules, assignments and approved usage |
| `design-system/layouts.md` | Breakpoints, reflow rules, page shells, grids |
| `design-system/components/` | `ui.`, `feature.` and `scene.` component specs |
| `design-system/motion/` | Motion source of truth: tokens, defaults and reduced motion, with values in both modes |
| `experience/screens/` | `screen.` specs |
| `experience/flows/` | `flow.` specs |
| `spec-plan.md` | The plan from `design-spec-plan`, if one was made; not a spec |
| `report/` | Audit register and decision log; not specs |

Spec names follow the naming rules: lowercase kebab-case, a type prefix, `.bundle` for bundles, unique across the project. Specs reference each other by spec name. To see what exists, list the folder (or its child pages).

[figma] Visuals live in Figma: read the files in `figma-access` using the read method listed. A component or screen is buildable when its spec is `ready` and Figma marks it Ready for dev.

[spec-only] There is no Figma. Visual styling values live in `tokens.md` and layout values in the `## Visual` section of `layouts.md`; each spec's `## Visual` section references them. A spec is buildable when it is `ready`.

---

## For agents

[platform]

<!-- On a documentation platform this page plays the AGENTS.md role (stubs/agents.md). -->

- Motion values, defaults and reduced-motion behavior live in `design-system/motion/` (`motion.tokens`, `motion.defaults`, `motion.reduced`). A Motion cell in a States table names a default; build it from those pages. `motion` also holds the motion levels, device tiers and what each change announces.
- [spec-only] [if applicable] Figma comps (`figma-comps`) are illustrations generated from the specs. Don't build from them; build from the specs.
- [figma] Before reading Figma nodes, load Figma's design-to-code guidance (its skill or connector resource), read only the nodes in each spec's `figma-nodes`, and treat Figma annotations and component descriptions as non-authoritative. Code Connect is optional.
