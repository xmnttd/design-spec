# Process Steps

The spec-driven loop, shown on every plan card: **six steps in three phases**, looping back to step 1. The spec is the source of truth at the center: **Write** creates it and **Update** keeps it current.

```
Architect      Author        Validate
1 Plan         3 Write       5 Review
2 Design       4 Build       6 Update  ──► back to 1
```

| Phase | # | Step | Skill / what happens |
|---|---|---|---|
| Architect | 1 | Plan | `design-spec-plan`: this plan |
| Architect | 2 | Design | *Depends on visual source (below)* |
| Author | 3 | Write | `design-spec-authoring`: write the specs |
| Author | 4 | Build | Prototype, test or hand off. Suggested: `design-spec-preview` |
| Validate | 5 | Review | `design-spec-audit`: QA against the spec, end-of-day or before handoff |
| Validate | 6 | Update | `design-spec-authoring`: keep the spec current |

## Design row by visual source

Step 2 depends on where visuals live. Show the row for the plan's visual source; step 3 (Write) is always `design-spec-authoring`.

| Visual source | 2 Design: skill / what happens |
|---|---|
| `figma` | No skill. Visual design in Figma |
| `spec-only` | `design-spec-authoring`: visual decisions in `tokens.md` and Visual sections |
| Moving to Figma (some specs still spec-only; only in a re-plan that changes the visual source) | No skill for specs already in Figma; `design-spec-authoring` for the rest |

In `spec-only` mode, recommend `design-spec-preview` once the first visuals are written: it's the only way to see them.

## Notes

- Show all six steps, including ones with no skill, so people see the whole loop.
- Steps don't have to run in order. Start where the project is.
- **Loop back to 1 means "run the loop again", not "re-run the plan every time".** Ordinary updates go straight to authoring. Re-run `design-spec-plan` only for a big change (a new product or experience, a new visual source, a component library adopted or changed, generative UI added).
- Don't list spec statuses. Authoring owns them.
- **Build** is broad: whatever turns the spec into something real. It covers the designer's own ways of building, testing and previewing (prototypes, usability tests, `design-spec-preview` to check how the specs render), and handing off to engineering, who implement from buildable specs: `ready`, and Ready for dev in Figma in `figma` mode. `design-spec-preview` is the suggested skill, not the whole step; don't present Build as preview only.
