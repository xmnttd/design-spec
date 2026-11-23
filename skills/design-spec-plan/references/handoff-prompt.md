# Go-time Prompt

The prompt the designer copies to start `design-spec-authoring`. It sits at the bottom of the card. It points at the plan file rather than repeating it, so the plan stays the single source.

`[plan location]` is where the plan was saved: the full local path recorded at save (for example `~/Work/acme-specs/design/spec-plan.md`), or the `spec-plan` page on the documentation platform ("Read the spec-plan page at [plan location] ..."). Never hard-code a relative path.

## Default (new spec set)

```
Use design-spec-authoring. Read [plan location]; the Project context
table answers the setup questions, so skip any it covers. Use its
scaffold as the proposed structure. Write only the First slice; leave
Deferred items until I ask.
```

## Self-directed

```
Use design-spec-authoring in self-directed mode. Read [plan location];
the Project context table answers the setup questions. Use its scaffold
as the proposed structure, within its First slice. I'll tell you which
spec to write first.
```

Drop ", within its First slice" when the plan has no First slice.

## Platform pending (plan saved locally)

Use this when the documentation platform couldn't be reached and the plan was saved locally. `[platform root]` is the platform name and root from the plan's Endpoint row (for example `Confluence, Harbor / Design specs`).

```
Use design-spec-authoring. First push [plan location], unchanged, to the
platform as the spec-plan page at [platform root]. Then read it; the Project
context table answers the setup questions. Use its scaffold as the proposed
structure. Write only the First slice; leave Deferred items until I ask.
```

## Re-plan

```
Use design-spec-authoring. Read [plan location], especially the "What
changed" line. Propose the changed settings for intent.md and the specs
it adds, moves or retires, and confirm with me before editing. Don't
touch specs the plan doesn't mention.
```

## Rules

- Keep it to five lines or fewer.
- Put the plan location in backticks, exactly as saved (paths may contain spaces or commas).
- Include the First slice line only when the plan has a time-box and a **First slice**; otherwise drop it.
- Never paste values or a full scaffold into the prompt; the plan file holds them.
- Use the exact skill name `design-spec-authoring`.
