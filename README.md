# Design Spec plugin

Skills for writing design specs that designers, engineers, QA and coding agents can all build from, with Figma owning visuals or the specs owning them (`spec-only`).

| Skill | Role |
|---|---|
| `design-spec-plan` | Optional. Plans a spec set before writing and saves `spec-plan.md` |
| `design-spec-authoring` | Writes and edits specs; holds the shared rules |
| `design-spec-preview` | Renders specs (with Figma in `figma` mode) as a local, interactive visual check |
| `design-spec-audit` | Read-only QA; the only writer of the audit register |

**All skills are required and install together.** The shared rules (ownership, spec types, best practices, connections, endpoints) live once, in `skills/design-spec-authoring/`, and plan, preview and audit read them from there, so they stop if it's missing. Authoring works on its own and warns once if its siblings are missing.

**Modes.** Each spec set has one visual source, set in `intent.md`: `figma` (Figma owns the visuals; specs own behavior and point to Figma nodes) or `spec-only` (no Figma; the specs own the visuals through `tokens.md` and each spec's `## Visual` section). Specs live as local markdown or on a documentation platform, always outside code repositories.

**Outputs.** A spec set under `design/` (or the same tree on a documentation platform), an optional `spec-plan.md`, the `report/` pages (audit register and decision log), and a local `design-preview/` folder that sits outside the spec set.

| File or folder | Written by |
|---|---|
| `spec-plan.md` | `design-spec-plan` |
| Specs, `intent.md`, `report/decision-log.md` | `design-spec-authoring` |
| `report/audit-register.md` | `design-spec-audit` (designers may edit a finding's Status and Note) |
| `design-preview/` | `design-spec-preview` |

## Quick start

- Plan: "Use design-spec-plan to plan the specs for my new project."
- Authoring: "Use design-spec-authoring to write the specs."
- Preview: "Use design-spec-preview to preview my specs."
- Audit: "Use design-spec-audit to check my specs."

## Works with

- Claude (Cowork, Claude Code): `.claude-plugin/plugin.json`
- Cursor: `.cursor-plugin/plugin.json`

No MCP servers are bundled. For Figma, Figma's official plugin is recommended (it bundles the Figma connector and its design-to-code skill), but not required.

## Structure

```
design-spec/
├── .claude-plugin/plugin.json
├── .cursor-plugin/plugin.json
├── README.md
└── skills/
    ├── design-spec-plan/
    ├── design-spec-authoring/
    ├── design-spec-preview/
    └── design-spec-audit/
```
