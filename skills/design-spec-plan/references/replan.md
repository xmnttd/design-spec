# Re-planning

For an existing spec set when something major has changed: a new product or experience shares the design system, the visual source changes, a component library is adopted or changed, generative UI is added, scope expands a lot, or the designer says the structure no longer fits. Ordinary edits don't need a plan; they go straight to `design-spec-authoring`.

## Steps

1. **Read** `intent.md`, the existing `spec-plan.md` if there is one (at the `spec-plan` location in `intent.md`, if set), and the list of specs by type. Don't read spec contents beyond what you need to understand structure.
2. **Ask what changed**, in one message. Skip anything already said.
3. **Run the conversation** (`conversation-guide.md`) only for the topics that changed.
4. **Reflect and confirm** (`conversation-guide.md`, section 4), covering what changed. It is the required stop in SKILL.md, step 4.
5. **Produce an updated plan**: same template, with the re-plan header and **What changed:** line (`plan-template.md`, section 1): what's different from the previous plan and why, naming a library adopted or changed, or generative UI added. It is not part of "For authoring", which stays questions only. If the old plan exists, ask before overwriting it, and save to the same location.
6. **Scaffold = proposed structure for the change.** Show the folders plus only the files added, moved or retired, each marked (`plan-template.md`, section 3). Never move or delete specs; authoring does that. If `design-system/motion/` is empty or missing pages (compare with its `pages` field, or the set predates motion), show `motion/` as added; authoring offers to copy the pages and add Motion columns.
7. **Hand off** with the re-plan prompt (`handoff-prompt.md`).

## Don't

- Rewrite or reorganize specs.
- Change statuses or touch `report/`.
- Treat the re-plan as a status report. Status and drift belong to `design-spec-audit`.
