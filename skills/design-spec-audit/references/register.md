# The Audit Register

`report/audit-register.md` (or its documentation-platform page, at the location in `intent.md`) is the one place findings live. The audit is its only writer. Designers may edit two things by hand, in a finding's detail block: **Status** (to `accepted`) and **Note**. Everything else is rewritten by each run.

All findings are owned by design. How they're shared out within the design team is up to each team; the register doesn't track it.

The empty page comes from `../design-spec-authoring/stubs/audit-register.md`. Both `design-spec-authoring` (when it scaffolds a set) and the audit (when the register is missing, `run-process.md`, step 1) may create it.

---

## 1. Layout

A short index for scanning, then one block per finding, so long findings read as paragraphs on any platform. The audit's stored state comes last.

```
# Audit Register

*Maintained by design-spec-audit. Edit only the Status and Note lines in Details.*

**Last ID:** AUD-042

## Open

| ID | Severity | Spec | Status |
|---|---|---|---|
| AUD-001 | Important | `ui.icon-button` | open |

## Details

### AUD-001 · Important · `ui.icon-button`

The icon button has no hover state; every interactive component defines default, hover, active, focus and disabled (states checklist, BP §5).

`CON-8` · state=hover · first seen 2026-10-12 · last seen 2026-10-12

**Status:** open
**Note:**

## Resolved

| ID | Spec | Finding | Resolved | Decision |
|---|---|---|---|---|

## Run log

| Run at | Scope | Mode | Read | Version | New | Resolved | Reopened | Open | Couldn't check |
|---|---|---|---|---|---|---|---|---|---|

## State

*Audit-owned. Don't edit.*

| Spec | Hash | Status | Last covered |
|---|---|---|---|
| `ui.icon-button` | 9f3a61c2 | ready | 2026-10-12 17:30 |

**Implemented:** none
```

- **Open (index):** rebuilt from Details on every run, never edited by hand. Blocking first, then important, then minor; within each, by spec name; `accepted` findings last. The spec name links to the spec (its page on a documentation platform).
- **Details:** one block per open or accepted finding, in the index's order.
  - **Heading:** `ID · Severity · spec`.
  - **Finding:** one or two plain sentences: what was found, and what the rule expects, with the rule cited in brackets at the end.
  - **Meta line** (audit-owned): check ID, location (§2), on a documentation platform the spec's page ID or link, first seen, last seen. For STR-9 and LIF-1 it adds `changed after YYYY-MM-DD` (the last run that covered the old content; moved forward when a later change is found). For an accepted finding it adds the acceptance marker, `accepted YYYY-MM-DD` (§3). The audit uses the meta line to recognize the finding on later runs.
  - **Status:** `open` or `accepted`. Resolved findings leave Details and move to the Resolved table. Any other value typed by hand counts as `open`: the audit sets it to `open` with the Note "Status '<value>' read as open".
  - **Note:** the reason for an `accepted` deviation, or the audit's own short note ("Reopened: the spec changed after it was accepted").
- **Resolved:** findings resolved in the last 14 days, with a short summary of the finding (12 words or fewer) and the decision-log entry that closed it, if any ("spec removed" for a deleted spec). Older rows are removed; their IDs are never reused.
- **Run log:** one row per run, newest first, kept to the last 10 runs. **Run at** is the date and local time the run started (`YYYY-MM-DD HH:MM`). **Scope** is `full`, `incremental (n specs)` or `narrowed: …`. **Mode** is the `visual-source` the run used. **Read** lists what was read (specs; Figma files; the Figma file version if readable). **Version** is this plugin's version, read from `version` in `../../.claude-plugin/plugin.json` (or `../../.cursor-plugin/plugin.json`). **Reopened** counts accepted findings reopened and resolved findings that regressed. **Open** counts `open` findings, with accepted ones in brackets, for example `25 (+2 accepted)`. **Couldn't check** names the checks or sources that didn't run in this run. Setup limits (Ready for dev and the file version without REST) aren't listed here; they're reported once under "Not available in this setup".
- **State:** audit-owned, rewritten on every run; designers don't edit it. One row per spec:
  - **Hash:** a short content hash (the first 8 hex characters of SHA-256) of the spec's front matter and body, excluding the `last-updated` line.
  - **Status:** the spec's `status` when last covered.
  - **Last covered:** the **Run at** of the last run whose checks ran on that spec. This is the spec's baseline for the next run. A spec whose checks couldn't run keeps its old hash and Last covered, so the next run checks it again.
  - **Implemented:** the specs whose `status` was `implemented` when last covered, or `none` (LIF-1).

  A spec that no longer exists loses its row on a full sweep.

On a documentation platform, the same layout is one page with these five sections, in this order. Write the headings, tables and detail blocks as the platform's native headings, tables and paragraphs.

**An older layout** (for example a single wide Open table, or the earlier `ID · Date · Spec · Finding · Status` table): on the first write, convert it to this layout.

- Carry every existing finding over with its ID, status, note and dates (a date-only row's date becomes First seen), drop any owner column, and set **Last ID** from the highest ID found.
- Infer each finding's check ID and location (§2) from its text. A row that can't be mapped keeps its ID with the check `legacy`: the audit never resolves it; it stays until the designer accepts it or removes it.
- Map old statuses: `resolved` or `fixed` go to Resolved; `wontfix` becomes `accepted`, with the Note "Converted from wontfix" before any old note; anything else is `open`.
- Rename old run-log columns to the current ones, add the new ones empty for earlier rows, and leave earlier rows' values as they were. Add the State section.
- On the first run after conversion, match findings by spec and description: a detected finding that describes the same problem on the same spec takes the converted finding's ID, and its key is corrected.

---

## 2. Finding IDs and keys

- **ID:** `AUD-` and a number, assigned once from **Last ID** + 1, never reused, even after its row is pruned from Resolved. Decision-log entries and designers cite it ("resolve AUD-012").
- **Key** (how a run recognizes the same finding): spec identity + check ID + location. Spec identity is the page ID on a documentation platform (from the meta line), or the spec name locally. The key is matched from the spec in the detail heading and the check and location in the meta line.
- **Renames:** on a documentation platform the page ID survives a rename, so findings carry over. Locally, if the decision log records the rename (old name → new name), carry the findings over to the new name; otherwise the old findings resolve and new ones open under the new name.
- **Deleted specs:** on a full sweep, findings on a spec that no longer exists move to Resolved with the Decision "spec removed", unless the decision log records a rename. Incremental runs leave them as they are.
- **Owning spec.** Each finding sits on one spec:
  - Cross-spec findings sit on the referencing spec, the one that holds the reference (REF-1, REF-2, REF-4, REF-5, REF-6, REF-8, GEN-1, GEN-3, GEN-4, GEN-7, LIB-5, LIB-6). GEN-4 sits on the spec whose slot adds the third level or closes the cycle.
  - Set-level findings sit on a fixed owner: `intent` (STR-8, GEN-9, LIB-1), `decision-log` (LIF-2), `tokens` (FIG-11), `motion` (MTN-6, MTN-8).
- **Locations** are canonical, never line numbers or free text:

| Location | Written as | Checks |
|---|---|---|
| Section (the default) | The exact section heading, for example `## States` | Every check not listed below |
| Criterion | `AC-n` | CON-7, GEN-8 |
| Front matter | The field name, for example `last-updated`; `front matter` for the whole block | STR-1 to STR-7, STR-9, LIF-1 (`status`), FIG-1, FIG-2, FIG-6, OWN-7, LIB-1, MTN-6 |
| State | `state=<value>`, as the spec names it | CON-8 and A11Y-1 (a missing state), FIG-3, MTN-3, MTN-4, MTN-10, MTN-11, MTN-12 |
| Figma | The Figma variant, property, variable, collection or node name | FIG-4, FIG-5, FIG-7 to FIG-11, LIB-6, MTN-7 |

- **One finding per section, listing every item:** CON-2, CON-3, CON-5, CON-9, CON-10, CON-11, CON-12, REF-1, REF-2, REF-4, REF-5, REF-7, REF-8, OWN-1, OWN-2, OWN-4, OWN-5, OWN-6, MTN-1, MTN-2, MTN-9, GEN-1, GEN-3, LIB-5, BLT-1, and FIG-9 (per node). The Finding lists every item found there, so the key stays the same as items are fixed or added. Every other check gives one finding per location.
- **Regressed:** a re-detected finding whose key matches a row in Resolved reopens under its old ID (back to Details, `open`), and is reported as "Regressed". Once the row is pruned, a new ID is used.

---

## 3. Reconciling

**First, the decision log** (every run, whatever the scope):

- **Renames** (old name → new name) carry findings over (§2).
- **`Accepted:` entries:** an entry that cites a finding's ID, with Change starting `Accepted:`, applies once: only when it's dated on or after the previous run's date and the finding doesn't already carry a marker with that date. It sets the finding to `accepted`, with the entry's reason as its Note and the marker `accepted <entry date>` in the meta line. Authoring never writes the register, so this is how an acceptance made there arrives. An older entry is never applied again, so a reopened finding stays open until a new acceptance. When a finding is reopened, the meta line records `reopened <date>`; an entry dated on or before that date is never applied to it.
- **LIF-1:** an entry for the spec dated on or after the finding's `changed after` date resolves it.

For each finding detected in this run:

- **Already open (same key):** update Severity, Finding and Last seen. Keep Status and Note as the designer left them.
- **Accepted (same key):** keep it accepted and update Last seen, unless it changed after it was accepted:
  - **Spec findings:** the spec's hash differs from its State row, and the acceptance was already in place at the previous run (an acceptance applied in this run stands).
  - **Figma findings** (FIG-): only when the compared structure at that location changed, that is, this run finds something different there from what the finding records.

  Then reopen it (`open`), remove the marker, and set the Note to "Reopened: changed after it was accepted" with the old reason kept after it. An `accepted` finding with no marker (set by hand, or the marker was removed) is stamped with this run's date, and the report says so. An `accepted` finding with an empty Note stays accepted, and the report asks for a reason.
- **In Resolved (same key):** reopen it as Regressed (§2).
- **New:** add a detail block with the next ID, First seen and Last seen today, Status `open`.

For each open or accepted entry not detected in this run:

- **The check ran on that spec:** move it to Resolved with today's date. If a decision-log entry cites its ID, link it in Decision.
- **The check didn't run** (out of scope, narrowed run, couldn't check, or skipped by the version shortcut in `figma-checks.md`): leave it unchanged. Not detected is not fixed.
- **STR-9 and LIF-1** resolve only on their own condition (`last-updated` on or after the recorded date; a decision-log entry dated on or after it), never because the hash no longer differs.
- **`legacy` findings** are never resolved by the audit.

A finding the designer deleted by hand is treated as unknown: if the problem is detected again, it returns under its old ID if that ID is still in the register, otherwise a new one.

Then: rebuild the Open index from Details, prune Resolved rows older than 14 days, update **Last ID**, add the run log row (pruned to 10), and rewrite the State section.

---

## 4. Writing

- Write only the register, only at the location in `intent.md` (or the location found or agreed in step 1 of `run-process.md`).
- **Check its version immediately before writing:** the file's modified time or content hash locally, or the page version on a documentation platform. If it changed during the run, re-read it, keep the designer's Status and Note edits, and redo the reconcile on the fresh copy. Never overwrite a designer's edit.
- Re-read after writing to confirm the index, detail blocks, tables and State section are intact, and that every open or accepted finding appears in both the index and Details.

---

## 5. How findings close

A finding is closed by a decision, never by the audit:

1. The designer resolves it in `design-spec-authoring` ("resolve AUD-012"). Authoring changes the spec and logs the decision with the finding ID.
2. Or it's fixed in Figma (renames, missing variants).
3. Or the designer accepts it as intentional: Status `accepted` with the reason in Note (in the finding's detail block), set in the register by hand or through an `Accepted:` decision-log entry from authoring.

The next run sees that the finding no longer applies and moves it to Resolved, linking the decision.

**Edits to `implemented` specs** (LIF-1): when there is no decision-log entry for the change, the finding asks the designer for the reason. It stays open until authoring logs an entry for that spec dated on or after the change. **A stale `last-updated`** (STR-9) stays open until the date is fixed.
