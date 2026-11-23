# Reporting

The chat message at the end of a run. The register holds everything; the message covers only what changed in this run.

---

## Nothing changed

One line:

> No changes since the last audit (YYYY-MM-DD HH:MM). 14 open (+2 accepted), 1 blocking.

That line is the whole message. Setup limits, migration notes and the motion baseline are mentioned on full sweeps only.

Every run adds a run-log row, even one with nothing in scope (`incremental (0 specs)`).

---

## Something changed

1. **Summary line:** scope and counts. "Incremental audit (4 changed specs, 6 neighbors): 3 new, 2 resolved, 1 reopened. 15 open (+2 accepted), 1 blocking."
2. **New**, blocking first: a short table with ID, severity, spec, finding.
3. **Resolved:** one line each, with the decision that closed it if logged ("AUD-009 `ui.button` focus state: resolved, decision 2026-10-11").
4. **Reopened:** accepted deviations whose specs (or compared Figma structure) changed since they were accepted.
5. **Regressed:** resolved findings detected again, under their old IDs.
6. **Escalated:** findings whose severity rose since the last run, with why ("AUD-014 is now blocking: `screen.home` moved to `ready`").
7. **Needs a reason**, only when new in this run: `implemented` specs edited with no decision-log entry (new LIF-1 findings), and findings newly `accepted` with an empty Note. Also say when the audit stamped a missing acceptance marker.
8. **Couldn't check:** one line each, with why ("Figma checks for 3 specs: rate limit"). These findings were left as they were.
9. **Not available in this setup** (full sweeps only), if anything applies: Ready for dev without the REST API, and the file version when the read method doesn't return it, one line each with what would make it available. Optional; never ask the designer to change their setup.
10. **Close with:**

> Resolve findings in a `design-spec-authoring` session (for example "resolve AUD-012"), fix Figma findings in Figma, or mark one `accepted` in the register with a reason. The next audit picks up the change.

On full sweeps, also mention where it applies: pending migration specs (mid-migration), a newer motion baseline in the plugin than the project's, the count of unfilled shells, and the informational lines in `checks.md`.

---

## Full sweep and first run

On a full sweep, the summary also says why it was full ("first run", "7 days since the last full sweep", "plugin updated", "`intent.md` changed"). On the first run there's nothing to compare with: list all findings by severity (blocking and important in full; minor as counts by check, with the register for detail), then offer the end-of-day run (`run-process.md`, step 7).

---

## Style

Short and factual. No praise, no restating the register, no findings that aren't in it. Spec names in code format. If a run found blocking findings, say so in the summary line.
