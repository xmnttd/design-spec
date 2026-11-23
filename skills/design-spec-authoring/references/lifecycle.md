# Lifecycle

The authoring process for an **existing spec set**: editing specs, adding new ones, renaming and retiring them. A new spec set follows `work-path.md` instead.

Designers also edit specs by hand. Every rule here assumes a page may have changed since anyone last looked at it.

---

## Contents

- Entry
- Scope
- Motion baseline upgrade
- The safe-edit protocol
- Front matter on edit
- References
- Decision log
- The audit register
- After changes

---

## Entry

**Adopting existing specs.** If specs exist but there is no `intent.md`, adopt them first: ask only what the specs don't already show (endpoint and visual source, `work-path.md` step 0), create a minimal `intent.md` as in `work-path.md`, "Single-spec route" (front matter and Structure and routing), and checkpoint. Then continue below.

1. Read `intent.md` for settings (`visual-source`, endpoint, Figma access, report locations) and structure.
2. Read `spec-plan.md` from the `spec-plan` location in `intent.md` (if set; otherwise look beside `intent.md`). If a plan exists and `intent.md` has no `spec-plan` field, add it as part of the next edit to `intent.md`. If the plan has a "What changed" line (a re-plan), read it, propose the added, moved or retired specs as a scope list, and confirm before editing. Don't touch specs the plan doesn't mention.
3. Ask what is changing, unless the designer or the re-plan has already said.
4. If the change is large (many specs, or a structural change), ask: "This touches several specs. Do you want to review the affected ones together before I edit?"
5. **Check motion.** If `design-system/motion/` is missing pages (compare with this skill's `motion/` folder, or with the `pages` field), the spec set predates motion: offer once to copy the missing pages in unchanged and, with confirmation, add a Motion column to existing States tables, filled from `motion.defaults` (one checkpoint per spec, or a batch the designer names). If the pages exist, compare the project's `baseline` with this skill's: if the skill's is newer, mention it once and offer the upgrade ("Motion baseline upgrade" below). Never update motion pages automatically.

---

## Scope

- Only touch the specs the designer named. Never expand scope by inference.
- `spec-plan.md` belongs to `design-spec-plan` (`spec-types.md`, §4). When the structure needs a big change (a new product or experience, a new visual source), suggest re-running `design-spec-plan`.
- If a change requires updating other specs (for example, references to a renamed spec), list them and confirm with the designer before editing them.
- **Documentation platform:** the boundary is the root folder or page tree agreed at scoping. Creating and updating pages inside it is allowed; anything outside it is not.
- If a spec you write or edit references specs that don't exist yet, list them and offer to write them or create `draft` shells, as in `work-path.md` ("Missing referenced specs"). Never create them without asking.
- **Adopting or changing a library** in an existing set (setting or changing `components` in `intent.md`): check the library suits the platform `intent.md` describes, list which ui specs become library-backed, make sure the motion pages exist (copy them first, as in Entry step 5; if the designer declines, skip the motion question and say library motion can be set later), ask the library motion question as in `work-path.md` step 1, and treat each spec change as its own edit with a checkpoint. The library-motion edits to the motion pages get their own checkpoint, and the edited pages go to `draft` (`spec-types.md`, §7). When `components.version` changes, re-check each library-backed spec's deviations against the new version's docs, and mention that motion values naming library tokens may be out of date.
- **Required housekeeping counts as part of the change:** edits these rules require as a side effect (adding the `spec-plan` field, updating `intent.md` folder descriptions, references to a renamed spec) are in scope, and are listed at the checkpoint so the designer sees them.
- **Motion:** changing which default a state uses is an edit to that spec. Changing a value or a default in `motion/` is an edit to the motion pages, made only when the designer asks. It affects every spec that names it, so list those specs at the checkpoint. Status on edit: `spec-types.md`, §7. Predesigned styles (expressive, bouncy) aren't included in this version of the skill: if the designer asks for one, say so and offer to customize the pages directly.
- Adding a new spec to an existing set is allowed within that boundary, using the stubs and the checkpoint, as in `work-path.md`. Recommend and confirm its type (and bundle, if it has sub-components) first (`choosing-types.md`). Generative use blocks and Generated content sections follow `genui.md`.

---

## Motion baseline upgrade

Only when the designer accepts the offer from Entry step 5:

1. **Compare** the project's motion pages with this skill's `motion/` pages.
2. **List the changes** for the designer, page by page, and mark any that touch something the project has customized.
3. **Apply them with confirmation,** one checkpoint for the motion pages. Customized parts change only where the designer says so. Status on edit: `spec-types.md`, §7.
4. **Update `baseline`** in `motion` to the skill's version.

Pages the project never customized are replaced whole. Customized pages take the new sections, tokens and defaults, and keep their own values.

**From 1.2 to 1.3** the change is additive: the style vocabulary (new tokens and defaults at neutral values, behaving like the ones they stand in for), the `(moment)` form, the system-wide rules (overshoot policy, the hand, tiers, duration and size, motion levels with No motion, device tiers, announcements, the optics layer) and `style: none` in front matter. No name is removed or changed, so no spec needs an edit. Announcements lines are added to specs as they're next edited; the audit lists the gaps (MTN-16).

---

## The safe-edit protocol

For every spec, every time:

1. **Read it before writing,** in full.
2. **Check stub coverage.** Compare it with its stub. Add a missing section only when the current change needs it, and only with real content; never add an empty heading. Other gaps are left for the audit (`work-path.md`, "Two rules that apply at every step").
3. **Make the change surgically.** Change what needs to change, nothing else. Removing an acceptance criterion adds its ID to the `Retired:` line (`spec-types.md`, §9).
4. **Fix invalidated reasoning.** If the change makes another part of the same page wrong, update that part too.
5. **Check the version immediately before writing.** On a documentation platform, the page version; locally, the file's modified time or content hash. If it changed since you read it (for example, the designer edited it by hand), re-read and merge. Never overwrite.
6. **Write.**
7. **Verify after writing.** Re-read the saved spec and confirm the change landed as intended and the front matter is valid.
8. **Checkpoint.** Stop and wait for the designer, as in `work-path.md`.

---

## Front matter on edit

- **`last-updated`:** set to today on every saved change.
- **`status`:** `spec-types.md`, §7.
- **`figma-version`** (`figma` mode): `source-of-truth.md`, "Pointers".

---

## References

Specs reference each other by spec name (see `spec-types.md`).

- **Renaming a spec:** rename it in place (file or page, `title`, and every referencing spec). The spec keeps its status, and no `deprecated` copy is left behind. On a documentation platform, rename the page rather than creating a new one, so history and inbound links are kept. Renaming an `implemented` spec affects code that uses the old name, so log it in the decision log.
- **Retiring a spec:** set `status: deprecated` and add one line directly under the Role line naming its replacement, if any ("Replaced by `ui.x`"); remove it instead only if the designer asks. Then update every spec that references it.
- **Promoting a bundle part** to its own spec (because it is now used on its own): create the new spec, remove the part from the bundle's `parts:` and `## Parts`, and update references.

Updating referencing specs is part of the change: list them, confirm with the designer, and edit only within the agreed boundary. If a referencing spec is outside it, tell the designer.

---

## Decision log

`report/decision-log.md` is written by the authoring skill, because decisions and their reasoning surface during authoring.

- **Log an entry when** a change resolves an audit finding, adopts or changes a component library, reverses an earlier decision, is a substantive edit to (or rename of) an `implemented` spec, or the designer asks. This list is the one owner of the decision-log triggers.
- **One table row per entry** (`stubs/decision-log.md`): date, spec name, what changed, why, who decided, and the finding ID if there is one.
- **Who decided** is the person (or role, if the team prefers) the designer names, such as the designer, a client stakeholder or a product owner. If it isn't clear, ask at the checkpoint (alongside the status question, not before the edit) and write the entry once they answer; never guess.
- **Append only.** Never edit or remove earlier entries.
- Write only to the log location recorded in `intent.md`.
- Manual edits by the designer create no entry; the audit may prompt for a reason later.

Example:

| Date | Spec | Change | Reason | Decided by | Finding |
|---|---|---|---|---|---|
| 2026-10-02 | `feature.form-order` | Removed the save-draft action | Drafts are out of scope for v1 | Jordan Lee (client product owner) | AUD-014 |

---

## The audit register

The authoring skill never writes to `report/audit-register.md` after scaffolding it. It doesn't read it by default. When the designer says they are resolving specific findings ("resolve AUD-012"), read those entries for context, make the decisions into spec changes, and log each in the decision log with its finding ID. The audit marks them resolved on its next run.

- **Accepting a finding as intentional** ("AUD-012 is fine as is, because…"): don't change the spec. Log a decision-log entry citing the finding ID, with Change starting `Accepted:` and the reason. The audit sets the finding to `accepted` on its next run. (The designer can also set it directly in the finding's Status and Note lines in the register.)
- **Working through the register:** when the designer asks to go through open findings, read the register, propose an order (blocking first), and handle each as above, one checkpoint per spec.

---

## After changes

- In `figma` mode, if visuals changed in Figma, check that the affected specs' state and variant names still match.
- After significant changes, suggest `design-spec-preview` to re-check visually, and `design-spec-audit` to re-check consistency.
- If specs were added, removed or moved between folders, confirm the `intent.md` folder descriptions are still accurate.
