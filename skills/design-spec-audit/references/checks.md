# Checks

The catalogue. Each check has an ID (used in finding keys), the rule it enforces, a default severity, and the modes it applies to. Rules are cited by short names for the files in `../design-spec-authoring/references/`: **ST** `spec-types.md`, **BP** `best-practices.md`, **SoT** `source-of-truth.md`, **SO** `spec-only-mode.md`, **FH** `figma-hygiene.md`, `genui.md`, plus `lifecycle.md`, the stubs in `../design-spec-authoring/stubs/`, and the project's `motion` pages. Which spec a finding sits on, and its location: `register.md`, §2.

---

## Severity

- **Blocking:** engineering would build the wrong thing, or built code is diverging. A finding is blocking when its check says "Blocking", or when its check is marked "blocking on ready" and the spec it sits on is `ready` or `implemented` ("blocking on implemented": only `implemented`). Checks without a mark never escalate.
- **Important:** a clear inconsistency or missing required content that doesn't yet change what gets built.
- **Minor:** format and naming conformance, hygiene, bloat.

A finding's severity can change between runs (for example when its spec moves to `ready`); the report names escalations (`reporting.md`).

**One problem, one finding.** When several checks fit the same problem, use one, so its key stays stable across runs:

- A missing section that a more specific check covers: the specific check, not CON-1 (for example CON-4 for acceptance criteria, MTN-2 for the Motion column, LIB-2 for the Library block, GEN-1 for a missing Generative use block, GEN-5 for generation states and fallbacks, GEN-6 for worked examples, A11Y-3 for a scene's fallback).
- A missing focus state: A11Y-1 (not CON-8).
- A value written in a Motion cell: MTN-2 (not MTN-1, which covers values elsewhere).
- A TBD or blank in a Motion cell: MTN-2 (not CON-2).
- A state the checklist requires that Figma has but the spec lacks: the checklist check (CON-8 or A11Y-1); mention the Figma variant in the Finding. FIG-4 is for Figma variants that no rule requires.
- A component a screen cites that has no spec: REF-4 (REF-1 is for references written as spec names that don't resolve). A library component with no spec: LIB-5 (not REF-4).
- An unfilled stub section: CON-9 (not BLT-2).
- Restated content that also contradicts its source: REF-6 (not BLT-1).
- A state whose name differs from Figma's only in case or format (`Hover` and `hover`, `is-open` and `isOpen`): one FIG-3, noted as a rename (no FIG-4 for the Figma name).

**Applying the states checklist** (CON-8): follow BP §5 "wherever they apply". "Active" applies only to pressable controls (buttons, icon buttons, links, tappable cards and items, toggles), not to text fields or other inputs. Empty, loading and error apply to components that display or fetch data themselves; a state covered by an `intent.md` default counts as present unless the spec contradicts it.

Modes: **both**, **figma** (only when `visual-source: figma`, or for specs with `figma-nodes`), **spec-only**.

---

## Exemptions

- **Shells** (valid front matter only, `status: draft`, no body; BP §10, "Shells"): only the Structure checks run; no content checks. On full sweeps the report MAY give one informational count ("6 unfilled shells").
- **Deprecated specs:** only the Structure checks run; their content isn't checked. References to them are still REF-2.
- **Library-backed states:** the library component's built-in states count as present for CON-8 and A11Y-1 (BP §5).
- **Keyboard behavior** covered by a cited WAI-ARIA APG pattern counts as present for A11Y-1 (BP §7).
- **Behavioral and accessibility constants** (budgets, camera limits, debounce and timeout durations, minimum target sizes, input constraints) are never OWN-1 or OWN-2 findings (SoT, "Styling values vs. behavioral constants").
- **Motion values** in `motion.tokens` are expected in both modes and are never an ownership finding.
- **Mid-migration specs** (a spec in a `figma` set with `## Visual` and no `figma-nodes`; SoT, "Migration from `spec-only` to `figma`") are pending migration, not OWN-3 or OWN-7 findings. Their `## Visual` sections are checked with the spec-only rule (OWN-2) instead of OWN-1, and they're listed in the run log as pending migration.

---

## Structure

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| STR-1 | Spec in the wrong folder for its type | ST §4 | Minor | both |
| STR-2 | Spec name breaks the naming rules (lowercase kebab-case, type prefix, `.bundle`, unique) | ST §5 | Minor | both |
| STR-3 | Filename prefix doesn't match `type:` | ST §1 | Important | both |
| STR-4 | `title` doesn't equal the filename (or page title). Compare plain text only: ignore link formatting a platform added by auto-linking a dotted name (`feature.app`), and a project key prefix applied by the mapping (`endpoints.md`, "Page titles") | ST §6 | Minor | both |
| STR-5 | Front matter isn't valid YAML | BP §3 | Important | both |
| STR-6 | Front matter fields out of order, empty, or not defined for the type; a required subfield missing (for example `library-key` on a library file in `figma-access`) | ST §6 | Minor | both |
| STR-7 | `status` isn't one of the allowed values | ST §7 | Important | both |
| STR-8 | `intent.md` routing or folder descriptions don't match what the folders hold | `intent` stub, Structure and routing | Minor | both |
| STR-9 | The spec's content changed (its hash differs from the register's State section) but `last-updated` is earlier than the change, that is, earlier than the last run that covered the old content. The finding records that date and stays open until `last-updated` is on or after it. No stored hash means nothing to compare | `lifecycle.md`, "Front matter on edit" | Minor | both |

## Content

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| CON-1 | A required section for the type is missing | ST §10 and the type's stub | Important | both |
| CON-2 | Open questions, TBDs, "confirm before build" notes or gap flags in a spec | BP §1 | Important; blocking on ready | both |
| CON-3 | History or argument in a spec: history markers ("previously", "used to", "no longer", "was changed to") and rejected alternatives ("we chose X because…", "instead of Y, which…"). The `Retired:` line and a deprecated spec's `Replaced by` line are exempt | BP §1 | Minor | both |
| CON-4 | No acceptance criteria on a feature, scene, screen or flow spec | ST §9 | Important; blocking on ready | both |
| CON-5 | A criterion that isn't written as Given / When / Then, or whose Then has no uppercase MUST or MUST NOT. Structure only; whether it's checkable isn't judged | BP §8 | Minor | both |
| CON-6 | Criterion IDs: the same `AC-n` twice in a spec, a gap in the numbering that isn't on the `Retired:` line, or a retired ID reused | ST §9, "Criterion IDs" | Important | both |
| CON-7 | A criterion at the wrong level (a screen criterion about a component's internals) | ST §9, "Which level owns a criterion" | Minor | both |
| CON-8 | A required state from the states checklist is missing (interactive components; empty, loading, error where they apply) | BP §5 | Important | both |
| CON-9 | Stub leftovers: tags, unfilled `[placeholders]`, `YYYY-MM-DD`, stub comments | BP §10 | Minor | both |
| CON-10 | A BCP 14 word in lowercase ("must", "must not", "should", "should not", "may") in the Behavior, Accessibility or Acceptance criteria section | BP §2 | Minor | both |
| CON-11 | Flow tables: a step that doesn't name its next, a Next or From step that points at a step that doesn't exist, or a criterion label that cites a step or branch that doesn't exist | ST §9, "Flow labels"; ST §10, "Flow tables" | Important | both |
| CON-12 | Real data in a spec: email addresses or phone numbers that aren't obvious placeholders, credentials (keys, tokens, passwords), or customer names and figures marked as real or confidential. Placeholders are never findings | BP §2, "Placeholder data only" | Important | both |

## References

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| REF-1 | A reference to a spec that doesn't exist | ST §8 | Important; blocking on ready | both |
| REF-2 | A reference to a `deprecated` spec | ST §8 | Important; blocking on ready | both |
| REF-3 | A bundle part used outside its bundle (promote it) | ST §3 | Minor | both |
| REF-4 | A screen uses a component no spec covers | ST §8 | Important; blocking on ready | both |
| REF-5 | A flow step on a screen that has no spec | ST §8, §10 | Important; blocking on ready | both |
| REF-6 | A contradiction in one of these pairs: a feature and the ui component it builds on; a screen and the properties of the components it cites; a flow and the screens it passes through; a host's Generated content and the Generative use blocks of the components it allows. Other pairs aren't checked | SoT, "The rule" | Important; blocking on ready | both |
| REF-7 | An asset pointer whose recorded `version` differs from the asset's current version (when the asset location is readable) | ST §6, `assets` | Important | both |
| REF-8 | A full criterion ID (`spec/AC-n`) that doesn't resolve: the spec exists but has no such criterion, or the ID is on its `Retired:` line (a missing spec is REF-1) | ST §9, "Criterion IDs" | Important | both |

## Ownership

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| OWN-1 | A visual styling value written in a spec | SoT, "What a spec MUST NOT contain in `figma` mode" | Important; blocking on ready | figma |
| OWN-2 | A literal value in a `## Visual` section, or anywhere outside `tokens.md` and the `layouts.md` `## Visual` section | SO §1 | Important; blocking on ready | spec-only, and pending-migration specs |
| OWN-3 | A spec has both `figma-nodes` and a `## Visual` section | SoT, "Migration from `spec-only` to `figma`" | Important | both |
| OWN-4 | A token referenced that doesn't exist (in `tokens.md`, or in Figma; in a library-backed set, the library's own token names count as existing) | BP §4 | Important; blocking on ready | both |
| OWN-5 | A primitive referenced in a 2- or 3-tier set | BP §4 | Minor | both |
| OWN-6 | A behavior that depends on a constant (debounce, timeout, limit, budget) names it without its value | SoT, "Styling values vs. behavioral constants" | Minor | both |
| OWN-7 | A ui, feature or scene component or a screen with no `figma-nodes`, that is neither a shell nor pending migration | SoT, "Pointers" | Important | figma (`visual-source: figma` only) |

## Figma

Details of what is read and compared: `figma-checks.md`.

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| FIG-1 | A `figma-nodes` node doesn't exist (confirmed missing, not unreachable) | SoT, "Pointers" | Important; blocking on ready | figma |
| FIG-2 | `figma-nodes` lists an instance instead of a main component, set, section or frame | ST §6 | Minor | figma |
| FIG-3 | A state or variant the spec names has no matching Figma variant, matched as a variant value (`state=hover`) or a boolean property (`disabled=true`) | SoT, "When Figma is missing something"; BP §5 | Important; blocking on ready | figma |
| FIG-4 | A Figma variant or state the spec doesn't describe | BP §5 | Important | figma |
| FIG-5 | Component properties in Figma differ from the spec's Properties table (for a library-backed spec: the options it lists under "Used in this product" must exist on the kit component; kit options it doesn't use aren't findings): a Figma property (variant, boolean, text, instance swap, slot) missing from the spec, or a different name, type or option list. Spec properties Figma can't hold (handlers, data, accessibility-only) aren't compared | SoT | Important; blocking on ready | figma |
| FIG-6 | `status: ready` but the node isn't marked Ready for dev, or the reverse (only when Ready for dev is readable). For a library-backed spec, compare only its local override node; with no override, FIG-6 doesn't apply. | SoT, "Ready for dev and buildable" | Important | figma |
| FIG-7 | Figma annotations or component descriptions that restate spec content (drift risk; recommend removal) | FH, "Annotations are not a source" | Minor | figma |
| FIG-8 | Figma names or sections don't align with spec names (recommended renames). Library kit components keep the kit's names and are never flagged | ST §5, "Matching names in Figma" | Minor | figma |
| FIG-9 | Hard-coded values in referenced nodes that should be bound to variables or styles: fills, strokes, effects, corner radius, padding and gaps, and type properties | FH, "Bind to variables" | Minor | figma |
| FIG-10 | Slot content edited on a screen instead of set through properties | FH, "Slots hold structure, not screen data" | Minor | figma |
| FIG-11 | A Figma variable collection (not private) that isn't listed in `figma-nodes.collections` of `tokens.md` or `motion.tokens`, or documented in `tokens.md`. A library kit's collections aren't findings | BP §4 | Important | figma |

For a library-backed spec, kit variants the product doesn't use (not under "Used in this product") are never FIG-3 or FIG-4 findings.

## Accessibility (presence only)

Contrast and deeper audits belong to a dedicated accessibility review; recommend one, don't compute it.

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| A11Y-1 | An interactive component with no focus state or no keyboard behavior | BP §5, §7 | Important | both |
| A11Y-2 | An icon-only control with no accessible name; an image or scene with no text alternative | BP §7 | Important | both |
| A11Y-3 | A scene without a fallback (poster), text alternative or performance budget | `scene-component` stub | Important | both |

## Lifecycle

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| LIF-1 | A spec in the stored `implemented` set whose content changed (its hash differs from the State section), including one that has left the set, with no decision-log entry for that spec dated on or after the change (the last run that covered the old content). The finding records that date, asks the designer for the reason (engineering can't see what changed), and stays open until such an entry exists | `lifecycle.md`, "Decision log"; ST §7 | Important; blocking on implemented | both |
| LIF-2 | A decision-log entry cites a finding ID above the register's **Last ID** (every `AUD-n` up to Last ID counts as existing, even once pruned from Resolved) | `lifecycle.md`, "Decision log" | Minor | both |

## Motion

Against the project's `motion` pages (rules MOT-1 to MOT-14, "How specs reference motion", "Exceptions"). Audit check IDs here use `MTN-`, so they're never confused with the motion rules' own `MOT-` IDs.

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| MTN-1 | A raw duration, easing, distance, delay, scale or blur value outside `motion.tokens` (a component's own debounce, timeout or auto-dismiss is behavior and allowed) | MOT-1 | Important; blocking on ready | both |
| MTN-2 | A ui, feature or scene component or screen with no Motion column, or a Motion cell that is blank, TBD, holds a value, or isn't one of the allowed forms (`Default: <name>`, any `Default:` form with `(moment)` after a name, `Default: <name> + <name>`, `Default: <a>; exit: <b>`, `Platform: <what>`, `Library: <what>`, `Library: <a>; exit: <b>`, `None — <reason>`, `—`) | `motion`, "How specs reference motion" | Important | both |
| MTN-3 | A Motion cell names a default that isn't in `motion.defaults` (each name, for a `+` combination; `(moment)` is stripped before the lookup) | Same | Important; blocking on ready | both |
| MTN-4 | A `Default: Fallback`: one finding per use, as a candidate for a new default | `motion`, "When nothing fits" | Minor | both |
| MTN-5 | An exception that doesn't state, in its Behavior section, what it overrides and how it behaves when interrupted, or doesn't state its reduced-motion behavior (in Accessibility) | `motion`, "Exceptions"; ST §10 | Important | both |
| MTN-6 | Motion pages missing compared with the `pages` field, or `motion/` empty (a set from before motion) | `motion` front matter | Important | both |
| MTN-7 | `motion.tokens` declares a mirroring Figma collection, and a variable's name or value differs from the page. A change of the file's `figma-version` alone isn't a finding. No record means nothing to check | MOT-10 | Minor (the page wins) | figma |
| MTN-8 | The motion pages disagree with each other: a default with no row in `motion.reduced`, or a token a default uses that isn't in `motion.tokens`. At baseline 1.3 or later, also a token or default this plugin's pages at that baseline carry that the project's pages have removed (names are stable: `motion`, "Changing this system") | `motion` pages | Important | both |
| MTN-9 | Motion rules restated as acceptance criteria in a spec | `motion`, "Rules" | Minor | both |
| MTN-10 | A reference (`default`) row with `—` on a component that appears or disappears, loops, or shows a changing value (modal, alert, skeleton, progress bar), which should name its motion | `motion`, "How specs reference motion" | Important | both |
| MTN-11 | A variant switched at runtime (checked, selected, expanded, on/off) whose "Differs by" cell names no motion | Same | Important | both |
| MTN-12 | A screen state reached only by arriving that names `None` instead of the matching Route change default | Same | Minor | both |
| MTN-13 | A `Library:` Motion cell in a project whose Source line in `motion` doesn't record the library's built-in component motion as used | `motion`, "Using a component library" | Important | both |
| MTN-14 | Customized motion pages break a system-wide rule. Report each case: a role token (`state`, `enter`, `exit`, `move`, `canvas`, `steady`, `fade`) that overshoots (a bezier y value outside 0–1, a `linear()` point above 1); a `spring-*` easing not paired with the same `spring-*` duration; an exit, value, edge-attached, layout or effect row (opacity, color, blur) on a `spring-*` easing; a hand row (Press, Hover — lift, Selection — toggle, Drag — pick up) on neither `spring-hand` nor a tween of 150ms or less; blur on a screen, panel, scrim, image or video row | `motion`, "System-wide rules" (Overshoot policy, The hand) | Important | both |
| MTN-15 | A spec names a `--motion-optic-` token, describes an optical effect as the way a state is shown, or has an `Optics:` line other than `Optics: none` | `motion`, "Optics layer" (In specs); MOT-4 | Important | both |
| MTN-16 | A state uses a default listed under "Announcements" in `motion` (for Validation error, In-place content change and Reposition, only in the cases the table names), and the spec's Accessibility section doesn't say what is announced | `motion`, "Announcements" | Minor | both |

The project's `baseline` being older than this plugin's motion pages isn't a finding: mention it once in the run message, with the upgrade offered in `design-spec-authoring`. Before 1.3, MTN-14 to MTN-16 don't run: those pages have no system-wide rules, Optics layer or Announcements.

## Component libraries

When `components.source` in `intent.md` isn't `custom` (ST §2, "Library-backed ui components"; `../design-spec-authoring/stubs/ui-component.md`, Library block).

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| LIB-1 | `components` names a library without a pinned `version`, or without `docs` (with a read date when the docs aren't versioned), or without `base` where the library offers a choice | ST §6 | Important | both |
| LIB-2 | A library-backed ui spec without "Built on" or "Used in this product" | `ui-component` stub, Library block | Important; blocking on ready | both |
| LIB-3 | A library-backed spec that restates the library's docs (anatomy, built-in states, keyboard or ARIA) without deviating | SoT, "Component libraries"; BP §9 | Minor | both |
| LIB-4 | A deviation listed without its behavior, states or accessibility, or a library-backed spec without a Property map when other specs cite its properties | `ui-component` stub, Library block | Important | both |
| LIB-5 | A library component used on a screen or in a Figma frame with no library-backed spec | ST §2, "Library-backed ui components" | Important | both |
| LIB-6 | An option, variant or size used in a screen or Figma frame that isn't under "Used in this product" | Same | Important | both |
| LIB-7 | Copied library code (shadcn and similar): the spec says deviations exist in the code but lists none, or animation classes are neither tokenized nor recorded as `Library:` motion. Presence only; the audit doesn't read code | `ui-component` stub, Library block | Minor | both |
| LIB-8 | In a set that started with the library, a library-backed spec built on one library component whose name doesn't match that component's name (in kebab-case), or whose variant names differ from the library's, without the mapping in "Built on" or the Property map. Sets that adopted a library later (the decision log records the adoption) are exempt | ST §5, "Library-backed names" | Minor | both |

Informational, mentioned once on full sweeps, never a finding: Figma comps (`figma-comps` in a spec-only `intent.md`) older than the specs they show. The comps file is never read for Figma checks.

Informational, mentioned once on full sweeps, never findings: `components.version` older than the library's current release, and, after a `components.version` change, motion values in `motion.tokens` that name library tokens may be out of date.

## Generative UI

For AI-ready components (their Generative use blocks) and hosts with a `## Generated content` section (`genui.md`; ST §8, §9 and §10; the Generative use and Generated content blocks in the component and screen stubs).

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| GEN-1 | A host's Generated content lists a component that doesn't exist or is `deprecated` (not REF-1), has no Generative use block, or isn't available as `standalone` or `both` | `genui.md`, §3; ST §8 | Important; blocking on ready | both |
| GEN-2 | A data contract in a Generative use block that isn't strict: fields without a type, limit or required/optional, or no source for data that must never be invented | `genui.md`, §2; component stubs, Generative use | Important; blocking on ready | both |
| GEN-3 | A nested slot in a data contract uses a component that doesn't exist, isn't AI-ready, or isn't available as `nested` or `both` | `genui.md`, §4 | Important; blocking on ready | both |
| GEN-4 | Nesting deeper than two levels, or a cycle | `genui.md`, §4 | Important | both |
| GEN-5 | A host missing one of the states around generation (generating, streaming, complete, partial where the host offers Stop, invalid, refusal, error) or one of the fallbacks (component not allowed, data missing or invalid, limit exceeded, whole output unusable) | `genui.md`, §3; feature and screen stubs, Generated content and States | Important | both |
| GEN-6 | A host with fewer than 3 worked examples, or none for a refusal or a fallback | `genui.md`, §3 | Important | both |
| GEN-7 | A worked example that uses a component the host doesn't allow, breaks the host's composition rules (order, limits, pairing), or uses sample data that breaks a component's data contract | `genui.md`, §2 and §3 | Important | both |
| GEN-8 | A host's Generated content criteria written as fixed outcomes ("shows the usage chart") instead of rules that must always hold | ST §9 | Minor | both |
| GEN-9 | A set with Generated content whose `intent.md` has no Generative UI bullet in Rules and guardrails, or (when the product has an assistant) no Assistant voice section | `intent` stub | Important | both |
| GEN-10 | A Generative use block on a spec type that can't carry it (screen, flow, foundation) | ST §10, "Also, where they apply" | Minor | both |

Informational, mentioned once on full sweeps, never a finding: a host that holds a conversation with an AI assistant and lacks parts of the conversational states guidance (stop, error, refusal, AI label, feedback; `genui.md`, §5).

What the model generates at runtime is never checked: it isn't the record.

## Bloat

| ID | Check | Rule | Severity | Modes |
|---|---|---|---|---|
| BLT-1 | A near-verbatim restatement of another spec's or `intent.md`'s content (reference it instead). Paraphrase and summary aren't checked | BP §9 | Minor | both |
| BLT-2 | Empty or boilerplate sections that don't apply | BP §9 | Minor | both |
| BLT-3 | Variants that re-spec unchanged properties | BP §5 | Minor | both |

---

## Not checked

- Visual quality, layout fidelity or rendering: that's `design-spec-preview`.
- Contrast and full accessibility audits: recommend a dedicated review.
- Figma's own motion data (prototype transitions, smart animate): motion lives in `motion/`.
- `spec-plan.md` and `report/`: not specs.
