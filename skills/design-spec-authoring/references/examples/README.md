# Examples

Finished specs for a fictional product, **Stockroom**: an internal tool where employees request equipment and supplies, and operations staff approve and fulfil the requests. The scenario is invented; names, data and links are placeholders.

Read an example only when the input for a spec is loose (vague direction, a Figma file without variables, no source). When there is a concrete source to extract from, work from the source and the stub instead.

Examples show the shape and level of detail of a finished spec. Don't copy their content into a real project.

The set mixes `figma` and `spec-only` modes, and custom and library-backed components (as a `mixed` set would), for illustration; a real project uses one `visual-source`. Specs the examples reference but don't include are assumed to exist:

- **Foundations:** `intent`, `tokens`, `icons`, `layouts` and the motion pages.
- **ui:** `ui.chip`, `ui.count`, `ui.drawer.bundle`, `ui.icon-button`, `ui.inline-alert`, `ui.link`, `ui.page-header`, `ui.panel.bundle`, `ui.switch`, `ui.text-field`, `ui.toast`.
- **feature:** `feature.assistant-message`, `feature.assistant-suggestion`, `feature.catalog-item-card`, `feature.catalog-picker`, `feature.side-menu-request`.
- **screen:** `screen.request-confirmation`.

| Example | Type | Mode | Shows |
|---|---|---|---|
| `ui.button.md` | ui-component | `figma` | A complete ui component: usage, anatomy, properties, states, variants, accessibility, no visual values |
| `ui.badge.md` | ui-component | `spec-only` | The same structure in `spec-only` mode, with a `## Visual` section of token references, and a Generative use block (AI-ready) |
| `ui.date-picker.md` | ui-component (library-backed) | `spec-only` | A component built on a library (shadcn/ui): the Library block, only the options used, deviations, `Library:` motion, a Visual section with overrides only |
| `ui.modal.bundle.md` | ui-component (bundle) | `figma` | A bundle: `parts:`, a Parts section, and an ARIA pattern reference instead of re-specified keyboard behavior |
| `feature.panel-order-summary.md` | feature-component | `figma` | Builds on a ui component; real copy, data contract, analytics, component-level acceptance criteria, and a Generative use block (AI-ready) |
| `feature.assistant-panel.md` | feature-component | `figma` | A host of generative UI: a Generated content section (allowed components, composition rules, fallbacks, worked examples), the states around generation in its States table, and rules-that-always-hold criteria |
| `scene.hero-animation.md` | scene-component | `figma` | An interactive animation authored outside Figma: asset pointers, state-machine inputs, budgets, fallback |
| `screen.request.md` | screen | `figma` | Composition by spec name, screen-level states and composition-only acceptance criteria |
| `flow.submit-request.md` | flow | — | Steps and branches tables, postconditions, journey-only acceptance criteria |

How they connect: `flow.submit-request` passes through `screen.request`, which composes `feature.panel-order-summary` and uses `ui.button` and `ui.modal.bundle`. `feature.assistant-panel` allows `ui.badge` and `feature.panel-order-summary` in its generated content.
