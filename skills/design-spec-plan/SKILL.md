---
name: design-spec-plan
description: "Plans how a team will write design specs, before any spec is written: gathers project context in a conversation, recommends the spec scaffold, and produces a shareable plan card, a spec-plan.md file and a paste-ready prompt that starts design-spec-authoring. Use this whenever a designer or team is kicking off a spec-driven design project, asking where to start, what to spec, how to structure a spec set or scope it to a time-box, whether to use Figma or spec-only mode, whether to build on a component library, or how to plan generative UI, even if they don't say \"plan\". Also use to re-plan a spec set after a big change (new product or experience, visual source or library). Not for writing or editing specs (design-spec-authoring), rendering them (design-spec-preview) or auditing them (design-spec-audit)."
---

# Design Spec Plan

Helps a designer or team decide **how they will write specs for this project**, before any spec is written. It is a conversation, not a form. It ends with a plan the team can review, and a prompt that carries the answers into `design-spec-authoring`.

It is a spec plan, not a project plan. No schedules, task lists or status tracking.

---

## Part of the design-spec plugin

This skill reads the family's shared rules from `design-spec-authoring`. All paths in this skill are relative to this skill's folder, including those inside `references/`. Load each file only when the step that needs it comes up:

- `../design-spec-authoring/references/spec-types.md`: spec types, folder structure, naming (building the scaffold).
- `../design-spec-authoring/references/work-path.md`: what authoring asks and scaffolds (steps 0–2).
- `../design-spec-authoring/references/best-practices.md`: only its "Tiers" section, for the token tier recommendation (scope topic).
- `../design-spec-authoring/motion/motion.md`: only its "Using a component library" section, for the library-motion recommendation.
- `../design-spec-authoring/references/endpoints.md` and `connections.md`: where the plan is saved, the outside-repository check, saving to a documentation platform, and the light scan of a Figma link (`connections.md`, "Reading a Figma file").

**Before anything else,** confirm `../design-spec-authoring/references/spec-types.md` is readable. If it isn't, stop and tell the designer the design-spec plugin is incomplete: all of its skills must be installed together.

---

## Start here

1. **Check for an existing spec set.** Look for `intent.md` and `spec-plan.md` (locally or at the documentation-platform location the designer gives; if `intent.md` exists, its `spec-plan` field gives the plan's location).
   - **Neither:** new plan. Continue below.
   - **`spec-plan.md` but no `intent.md`:** a plan exists but authoring hasn't started. Offer to revise it; ask before overwriting it. Then continue below.
   - **`intent.md` exists:** this is a re-plan. Follow `references/replan.md`.
   - **No location given yet** (for example a bare invocation): don't ask for one before showing the overview. Run this check once a location comes up.
2. **Read what the designer gave you:** a brain dump, notes, a transcript, a brief, a PRD, Figma links. Extract answers to the topics in `references/conversation-guide.md`. For a Figma link, do the light scan (`references/conversation-guide.md`, topic C).
3. **Choose the entry path.**
   - **Invoked with no project material and no go-ahead** (for example just "Use design-spec-plan"): show `references/overview.md` exactly as written, then stop and wait.
   - **They reply "start my spec plan"** (or any go-ahead such as "start" or "walk me through it"): use the **walkthrough path**. Go topic by topic (`references/conversation-guide.md`, section 2), two or three questions at a time, in this order: context, what you're building, inputs, scope, spec approach, team. Recommend a default for each topic and let them correct it. Then continue to step 4.
   - **Material already given** (notes, transcript, a filled-in brain dump, or just a Figma link), in the first message or after the overview: skip the overview, say what you read (scan a lone Figma link first), and go to step 4.
4. **Reflect and confirm. This is a required stop,** even when the input is complete or it's a re-plan: a full brain dump shortens the questions; it doesn't remove the check. Send only the reflect-and-confirm message (`references/conversation-guide.md`, section 4), then wait. It covers what you heard; what you recommend (visual source, endpoint, guidance, token tier, breadth, components, library motion, generative UI and, if there is a time-box, the first slice); the platform-fit check when a library is used; and your questions.
   - **Do not produce the card, `spec-plan.md` or the prompt in this message,** no matter how complete the input is.
5. **After their reply:** apply corrections. If they confirm without answering every question ("looks right"), take your recommended defaults for the rest and list them under Assumptions on the card. If a blocking gap remains, ask one more short batch; otherwise continue.
6. **Produce the outputs** below, in the next message.

**Exception:** if the designer explicitly asks to skip the check ("just generate it"), produce the outputs and list your assumptions at the top of the card.

---

## How to run the conversation

- Conversational and flexible: topics can come in any order, and the designer can ask odd questions or go back.
- **Ask only what changes the output.** Every question must change the scaffold, the recommended steps or the handoff prompt.
- **Batch questions** into one message. Don't drip-feed.
- **Recommend, don't interrogate.** Where a sensible default exists, propose it with a one-line reason and let the designer correct it.
- **Treat unverified statements as assumptions.** When the input is a live discussion or a transcript, label what's inferred rather than decided.
- Explain a question briefly if the designer seems new to specs. Many are.

Topics, wording, defaults and skip rules: `references/conversation-guide.md`.

---

## Outputs

Produce all three. They are one plan at two levels: the card is the file's first part, so what you show in chat and what you save are the same.

1. **Plan card.** A scannable one-pager, like a postcard: short table cells, a copyable scaffold, all process steps, and the handoff prompt. **It must fit on one printed page (about 80 lines).** Show it in chat exactly as saved. Template and length limits: `references/plan-template.md`.
2. **`spec-plan.md`.** The card plus a short "For authoring" block (at most four open questions).
   - **Local markdown:** agree the project folder first (ask in step 4 if it wasn't given) and apply the outside-repository check in `../design-spec-authoring/references/endpoints.md` ("Outside code repositories"). Save to `[project folder]/design/spec-plan.md`. `design/` is the spec set's root, where authoring later creates every spec; create it if needed, but nothing else in it. Record the full path; every prompt uses it as `[plan location]`.
   - **Documentation platform:** create one page at the root the designer agreed, titled `spec-plan` under the page-title rules in `../design-spec-authoring/references/endpoints.md` (with the project key if the space is shared): the page is the only record, so don't keep a local copy. Its location is `[plan location]`. Reach the platform as `../design-spec-authoring/references/connections.md` describes; never ask for credentials in chat. If the platform can't be reached, save `spec-plan.md` in the session's working folder or a folder the designer names, outside any code repository. Don't create a `design/` folder: the spec set will live on the platform. Record the full path as `[plan location]`, tell the designer the plan isn't on the platform yet, and use the platform-pending prompt (`references/handoff-prompt.md`): authoring pushes the unchanged plan to the platform location first.
   - `spec-plan.md` is not a spec. Who writes and reads it: `../design-spec-authoring/references/spec-types.md`, §4.
3. **Go-time prompt.** At the bottom of the card. Templates: `references/handoff-prompt.md`.

If the designer wants to share it, offer only what this environment can produce (for example a one-page PDF, or text to paste into Slack). Don't expand the card to fill space.

How to derive the scaffold: `references/scaffold-guidance.md`. Process steps: `references/process-steps.md`. A worked example: `references/example-plan.md`.

---

## Hard rules

- **No visual values in the plan.** No hex codes, spacing or type sizes. Figma and PRD links appear as context only (source chips). In `figma` mode values live in Figma; in `spec-only` mode they live in `tokens.md` (layout values in `layouts.md`), written by authoring.
- **Don't ask for the component list.** Authoring establishes it later. The plan recommends spec *types* and foundation files, not individual components.
- **Don't ask about motion,** except the library-motion question when a component library has its own motion (`references/conversation-guide.md`, topic D). Motion is always in scope: authoring copies the skill's ready motion pages into `design-system/motion/` at scaffold, and the designer can customize them later. The plan only shows the folder in the scaffold.
- **Two endpoint options only:** **Local markdown** or **Documentation platform** (for example Confluence or Notion). If a platform, ask which one and where the spec set should live; if local, which project folder (outside any code repository); specs go in its `design/` folder.
- **Never write specs, audit or log decisions.** Those belong to the other skills. `report/` files are not the plan's to write.
- **No open questions or TBDs in specs, and none implied by the plan.** Unresolved items (undefined rules, unknown data sources) are *known unknowns*. List them in the plan under open points for authoring, as questions only. Never say they will go into specs; non-blocking gaps go to `design-spec-audit`.
- **No fabricated inputs.** If a file, link or fact wasn't provided, don't list it.
- **Don't invoke the next skill.** Recommend authoring and hand over the prompt; the designer decides when.

---

## Handoff

End by recommending **design-spec-authoring** as the next step and pointing to the prompt. Say what to expect: in guided mode authoring pauses after every saved spec; chat is only for blocking questions; non-blocking gaps are left for `design-spec-audit`. If the designer will be working with others, remind them to share the project so everyone sees the plan.

Plan is normally one-time. If something major changes later, suggest re-running it (`references/replan.md`); don't ask for updates to the plan for ordinary edits.

---

## The design-spec family

| Skill | Role |
|---|---|
| `design-spec-plan` | Optional. Plans a spec set before writing and saves `spec-plan.md` (this skill) |
| `design-spec-authoring` | Writes and edits specs; holds the shared rules |
| `design-spec-preview` | Renders specs (with Figma in `figma` mode) as a local, interactive visual check |
| `design-spec-audit` | Read-only QA; the only writer of the audit register |

The typical order is plan, author, preview, audit, but any step can be entered directly.
