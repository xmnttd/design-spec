---
name: design-spec-audit
description: "Read-only QA for an existing design spec set (one with intent.md): checks the specs, and Figma in figma mode, for drift, contradictions, broken references, naming and format violations, missing states and acceptance criteria, motion issues and bloat, then maintains the audit register and reports only what changed since the last run. Use this whenever a designer wants to audit, check, validate or health-check their specs, run spec QA or review specs, before handoff or after changes, find what's out of sync with Figma, or see what's still open, even if they don't say \"audit\". Not for writing or fixing specs (design-spec-authoring), rendering a visual preview (design-spec-preview) or planning a new spec set (design-spec-plan).\n"
---

# Design Spec Audit

A read-only check of a spec set against the family's rules, and against Figma in `figma` mode. Findings go into one place, the audit register, so specs stay free of open questions and argument. Each run reconciles with the last one and reports only what changed. Findings close when the specs are fixed (through `design-spec-authoring`) or when a designer accepts a difference as intentional.

This file is the summary and router. Each section points to the reference file that holds the full rule; when the two seem to differ, the reference file wins.

---

## Part of the design-spec plugin

This skill checks against the rules `design-spec-authoring` holds and reads them from that skill's folder. All paths in this skill are relative to this skill's folder, including those inside `references/`.

- `../design-spec-authoring/references/source-of-truth.md`: who owns each fact, by mode.
- `../design-spec-authoring/references/spec-types.md`: types, names, folders, front matter, statuses, references, acceptance criteria, required content.
- `../design-spec-authoring/references/best-practices.md`: specs as decisions, states checklist, accessibility, criteria quality, lean specs, stub cleanup.
- `../design-spec-authoring/references/figma-hygiene.md`: Figma file recommendations (`figma` mode).
- `../design-spec-authoring/references/genui.md`: generative UI (Generative use blocks, hosts' Generated content, nesting) and conversational states for hosts of an AI assistant.
- `../design-spec-authoring/references/lifecycle.md`: front matter on edit, the decision log.
- `../design-spec-authoring/references/spec-only-mode.md`: how visuals are written in `spec-only` mode.
- `../design-spec-authoring/references/connections.md`: reaching Figma and documentation platforms, read-only rules.
- `../design-spec-authoring/references/endpoints.md`: how specs map to documentation-platform pages.
- `../design-spec-authoring/stubs/`: the template for each type, and `audit-register.md` for the register.

The project's motion pages aren't part of that check; missing pages are MTN-6.

**Before anything else,** confirm those files are readable. If they aren't, stop and tell the designer the design-spec plugin is incomplete: all of its skills must be installed together.

**A check exists only where a rule exists.** Every finding cites the rule it breaks. If something looks wrong but no rule covers it, it isn't a finding.

---

## Start here

1. **Find the spec set** and read `intent.md` (`references/run-process.md`, step 1). No `intent.md` means there is nothing to audit: stop and suggest `design-spec-authoring`.
2. **Read the register, its run log and State section, and the decision log** to decide the scope: a full sweep or an incremental run (`references/run-process.md`, step 2).
3. **Connect read-only** to Figma (`figma` mode) and the documentation platform, if used (`references/figma-checks.md`).
4. **Run the checks** in `references/checks.md` that apply to the mode.
5. **Reconcile and write the register** (`references/register.md`).
6. **Report the changes in chat** (`references/reporting.md`). After the first run, offer once to schedule an end-of-day run.

A run needs no opening question. Say the scope in one line and go, unless the designer has asked for something narrower ("just the components").

---

## What to load, and when

| File | Load when |
|---|---|
| `references/run-process.md` | Always |
| `references/checks.md` | Always |
| `references/register.md` | Always |
| `references/reporting.md` | Always, before the final message |
| `references/figma-checks.md` | `visual-source: figma`, or any spec has `figma-nodes` |
| `../design-spec-authoring/references/source-of-truth.md` | Always |
| `../design-spec-authoring/references/spec-types.md` | Always |
| `../design-spec-authoring/references/best-practices.md` | Always |
| `../design-spec-authoring/references/lifecycle.md` | Always (STR-9, LIF-1, LIF-2) |
| The project's `design-system/motion/` pages | Always (the MTN checks; missing pages are MTN-6) |
| `../design-spec-authoring/references/spec-only-mode.md` | `visual-source: spec-only`, or any spec has a `## Visual` section |
| `../design-spec-authoring/references/figma-hygiene.md` | `visual-source: figma`, or any spec has `figma-nodes` (FIG-7, FIG-9, FIG-10) |
| `../design-spec-authoring/references/genui.md` | The set has Generative use blocks, Generated content sections or a host of an AI assistant (the GEN checks and the conversational-states line) |
| `../design-spec-authoring/references/connections.md` | Always in `figma` mode; or connecting to a documentation platform; or a connection fails |
| `../design-spec-authoring/references/endpoints.md` | The spec set lives on a documentation platform |
| `../design-spec-authoring/stubs/intent.md` | Always (STR-8, GEN-9) |
| `../design-spec-authoring/stubs/scene-component.md` | The set has scene specs (A11Y-3) |
| `../design-spec-authoring/stubs/<type>.md` | Checking a spec's required content |

---

## Non-negotiables

- **Writes only the audit register (and, on the designer's approval, a scheduled task).** Never specs, `intent.md`, the decision log, `spec-plan.md` or Figma. It never fixes anything itself (`references/register.md`).
- **Read-only in Figma.** Read-only tools, or plugin scripts that only read; never Code Connect mapping tools; only the methods in `connections.md` ("Only these methods"), which owns these rules (`references/figma-checks.md`).
- **"Couldn't check" is never "checked."** A check that couldn't run leaves its findings as they were and is reported; it never resolves anything (`references/register.md`).
- **Stable findings.** The same problem keeps the same ID across runs; designers' status and note edits are kept (`references/register.md`).
- **Report the change, not the state.** The register holds everything; the chat message covers only what changed (`references/reporting.md`).
- **Facts, not opinions.** Each finding states what was expected (the rule), what was found, and where.
- **Never ask for credentials in chat** (`../design-spec-authoring/references/connections.md`).

---

## The design-spec family

| Skill | Role |
|---|---|
| `design-spec-plan` | Optional. Plans a spec set before writing and saves `spec-plan.md` |
| `design-spec-authoring` | Writes and edits specs; holds the shared rules |
| `design-spec-preview` | Renders specs (with Figma in `figma` mode) as a local, interactive visual check |
| `design-spec-audit` | Read-only QA; the only writer of the audit register (this skill) |

The typical order is plan, author, preview, audit. Run the audit before handoff and after significant changes; an end-of-day run keeps the register current. Findings are fixed in `design-spec-authoring` (for example "resolve AUD-012") or in Figma.
