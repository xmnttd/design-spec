# Run Process

The steps of one audit run, in order.

---

## Contents

- Step 1 — Find the spec set
- Step 2 — Decide the scope
- Step 3 — Connect
- Step 4 — Run the checks
- Step 5 — Reconcile and write the register
- Step 6 — Report
- Step 7 — Offer an end-of-day run (first run only)

---

## Step 1 — Find the spec set

1. Find `intent.md`, locally or at the documentation-platform location the designer gives. If there's none, stop: there is no spec set to audit. Suggest `design-spec-authoring`.
2. From `intent.md` front matter, read `visual-source`, `endpoint`, `figma-access`, `audit-register` and `decision-log`. The mode lives in `intent.md` only.
3. **The register:** read it from the `audit-register` location. If `intent.md` has no `audit-register` field but `report/audit-register.md` exists, use it, and remind the designer once to record the field in `intent.md` through `design-spec-authoring`. If there is no register at all, offer once to create it at `report/audit-register.md` from `../design-spec-authoring/stubs/audit-register.md`, and tell the designer to record the location in `intent.md` through `design-spec-authoring` (the audit never edits `intent.md`). Without a register, run the checks and report in chat only.
4. **The decision log:** read it (read-only) on every run, whatever the scope. It carries renames and `Accepted:` entries, links resolved findings to their decisions, and shows which edits to `implemented` specs were logged (`register.md`, §3).
5. List every spec by folder, with front matter. Skip `report/`, `spec-plan.md`, `AGENTS.md` and any `README.md`: they aren't specs, and their lack of front matter isn't a finding. If a README or note says the set is a frozen copy or that the record lives elsewhere, and `intent.md` disagrees, mention it in the report on full sweeps; audit what `intent.md` points to.

---

## Step 2 — Decide the scope

Read every spec and compute its content hash (`register.md`, §1, State). A spec has **changed** when its hash differs from its State row, or it has no row (a new spec). Modified times and page versions are only hints: syncs such as OneDrive change modified times without changing content, so the hash decides.

**Full sweep** when any of these is true:

- There is no run log (first run), or the register has no State section (an older register).
- The last full sweep was more than 7 days ago.
- The plugin version differs from the one in the last run-log row. Read it from `version` in `../../.claude-plugin/plugin.json` (or `../../.cursor-plugin/plugin.json`).
- `intent.md` changed (including `visual-source`).
- The designer asks ("full audit").

**Incremental run** otherwise:

- **Changed specs:** each spec's baseline is the last run that covered it (its Last covered). A spec is in scope when its hash differs from that run's. Specs that couldn't be checked keep their old hash, so they're carried into the next run.
- **Neighbors:** specs a changed spec references, and specs that reference it. A change to `tokens.md`, `icons.md` or `layouts.md` makes every spec that references the changed names a neighbor. A change to a motion page makes every spec whose Motion column names an affected default a neighbor.
- **Cross-spec checks** (contradictions, broken or deprecated references) always run for changed specs and their neighbors.
- **Set-wide checks** run when their inputs change: STR-2 uniqueness (a spec added or renamed), STR-8 (the folder contents), LIF-2 (the decision log), FIG-11 (`tokens.md`, `motion.tokens` or the Figma file version), MTN-8 and MTN-14 (any motion page), GEN-9 (a Generated content section added or removed), LIB-1 (`intent.md`, which already makes the run full).
- **The decision log** is processed on every run (`register.md`, §3).
- **Figma:** in `figma` mode, Figma-only changes (a variant renamed in Figma, nothing changed in the specs) are found by the structural compare, which runs on changed specs and on every full sweep. If the file version is readable and has changed since the last run, compare every referenced node instead (`figma-checks.md`). A compare skipped by the version shortcut counts as "not run": its findings are carried over unchanged.

If the designer narrows the scope ("just the components", "only `screen.home`"), audit only that; cross-spec checks still look at its neighbors. Narrowed runs are logged as such and never resolve findings outside their scope.

Say the scope in one line before checking, for example: "Incremental run: 4 changed specs and 6 neighbors, plus Figma for those nodes."

---

## Step 3 — Connect

- **Spec set:** read-only access to the folder or the documentation-platform root (`connections.md`). Write access is needed only for the register.
- **Figma** (`figma` mode): as `figma-checks.md` describes.
- Connect only to what the scope needs: an incremental run with nothing in scope needs no Figma connection. If something can't be reached, continue with what can: every check that depends on it is "couldn't check" for this run (`register.md`, "Reconciling").

---

## Step 4 — Run the checks

Run every check in `checks.md` that applies to the mode and scope, with its exemptions (`checks.md`, "Exemptions"). For each finding, collect: the spec it sits on (and its page ID on a documentation platform), the check ID, the location (`register.md`, §2), what was expected (citing the rule), what was found, and the severity.

- One finding per problem per location. The same problem in several modes or variants is one finding that lists them.
- Record each check that couldn't run, and why (Figma unreachable, a page unreadable, a value only the REST API exposes).
- **Mid-migration** (`figma` set, some specs still with `## Visual` and no `figma-nodes`): those specs are pending migration, not findings for it. Check their `## Visual` sections with the spec-only rule (OWN-2), and list them in the run log as pending migration.

---

## Step 5 — Reconcile and write the register

Follow `register.md`: process the decision log, match findings to existing entries by key, add new ones, resolve those no longer detected (only where the check ran), keep accepted deviations, prune Resolved, append to the run log, and rewrite the State section. Check the register's version immediately before writing; if it changed during the run (a designer edited it), re-read and merge.

---

## Step 6 — Report

Report in chat as `reporting.md` describes: what's new, resolved, reopened, regressed and escalated, what couldn't be checked, and which new `implemented` edits need a reason.

---

## Step 7 — Offer an end-of-day run (first run only)

After the first run on a spec set (no earlier run log), and only where the environment can schedule tasks, offer once:

> "Want me to run this audit automatically at the end of each workday? It only reports when something changed."

If the designer agrees, create a weekday task at the end of their working day (ask the time if it isn't obvious; suggest 17:30 local), with a prompt like: "Use design-spec-audit on the spec set at `[location]`. Run the audit as usual; report only changes." Never create it without a yes, and don't offer again on later runs.
