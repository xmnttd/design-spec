# Connections

How the design-spec skills reach Figma and documentation platforms, what they do when they can't, and what they may write. This file holds the full text of "Only these methods", credentials, "couldn't check", read-only scripts and Code Connect; other files point here. Where and how specs are written is in `endpoints.md`.

---

## Contents

- Access methods
- Setting up
- Reading a Figma file
- First-connection check
- Writing to Figma
- When a connection is unavailable

---

## Access methods

Recommend a method; don't enforce one. Use whatever is available and works.

| Target | Interactive agent session | Scripted or no connector | Fallback |
|---|---|---|---|
| Figma | An official Figma connector (MCP) | Figma REST API with a read-only personal access token | Exported tokens or variables file (a W3C DTCG `tokens.json` is accepted), screenshots for reference |
| Documentation platform | The platform's connector (MCP), if one exists | The platform's REST API with an API or integration token | Manual copy and paste, or markdown export and import |

Examples of documentation-platform connectors: an Atlassian connector for Confluence, a Notion connector for Notion. Treat these as examples; the same approach applies to any comparable tool.

**Code Connect** (mapping Figma components to code) is optional. It helps engineering but isn't needed to author specs, and the mapping lives in Code Connect or the codebase, never in a spec (`source-of-truth.md`). Setting it up writes to Figma, so it's an engineering task outside these skills: never call Code Connect tools that write mappings. If the connector asks its Code Connect question, relay it word for word once per session, say it's set up outside these skills, and continue read-only.

**Figma's official plugin.** If the designer's environment has Figma's official plugin installed (it bundles the Figma connector and its own usage skills), recommend using it: it typically handles connection setup, and its skills define the correct way to call Figma's tools, including required steps before writing. This skill decides **what** to read or write in Figma; the plugin's skills govern **how**. Follow them whenever they apply. The plugin is recommended, never required: everything here also works with a plain connector or the REST API.

**Only these methods.** Use and recommend only the methods in the table above: Figma's official connector (or plugin), the REST API, or an export. If other Figma tools happen to be installed (third-party connectors or bridge plugins), don't rely on them, don't mention them, and never ask the designer to set up or re-authorize them.

### Figma variables over REST

Figma's Variables REST API is available only on Enterprise plans, and the personal access token needs the `file_variables:read` scope as well as `file_content:read`. Without it, read variables through a connector, or use an exported tokens or variables file (a DTCG file is accepted) and record `read-method: export` in `intent.md`. Say which method was used; never treat a file read without variables as a variables check.

---

## Setting up

1. **Detect** what is already connected before asking the designer for anything.
2. If nothing suitable is connected, **say what's missing and offer setup guidance**: which connector or API to use, where the designer enables it, and what access it needs (read access to the Figma file; read and write access to the agreed root on the documentation platform).
3. **Never ask for tokens, keys or passwords in chat.** Credentials stay in the designer's connector or environment settings.
4. Record the Figma file(s) and preferred read method (`MCP`, `REST` or `export`) in `intent.md` (`figma-access`), so engineers' agents know how to read what the specs point to. For each file, record its role: `library` (components and variables), `composition` (screens), or both when one file holds everything, plus the published library key for library files.

### Multiple Figma files

- ui, feature and scene specs point at the library file; screens point at the composition file. Each spec's `figma-nodes.file` says which.
- `tokens.md` points at the library file's variable collections.
- `figma-version` is recorded per spec against the file that spec points to.
- A screen cites the components it uses by spec name; their visuals are read from the library through their own specs, not re-described from the composition file.
- Scope library searches to the project's library key; unscoped searches can return other teams' libraries.

---

## Reading a Figma file

### Who reads what

The design-spec skills read Figma to plan, write and check specs. Engineers' coding agents build from the specs, so they read far less.

| Reader | Reads |
|---|---|
| `design-spec-plan` | A light scan: every page with its top-level sections, and the variable collections with their modes. No values, no component lists |
| `design-spec-authoring` | What the specs being written need: pages, sections, components and variants, full variable collections and modes, styles, Ready for dev, file version |
| `design-spec-preview` | Only the nodes in specs' `figma-nodes`, what those nodes need to render, the collections `tokens.md` lists, and the styles the read nodes use (`../design-spec-preview/references/figma-mode.md`) |
| `design-spec-audit` | Only the nodes in specs' `figma-nodes` (with their variants, properties, instances, annotations and descriptions), the collections `tokens.md` and `motion.tokens` list, the names of all local collections (FIG-11), and Ready for dev and the file version where readable (`../design-spec-audit/references/figma-checks.md`) |
| Coding agents (building) | Only the nodes and Figma references written in the spec files they are building from (`stubs/agents.md`). They never browse the file |

### Seeing the whole file

- **A link opens one page or node.** Reading it returns only that page, so start by listing every page in the file and its top-level sections. Never conclude what a file holds from the page a link opened.
- **Variables from one node aren't the collections.** Variables read through a node show only what that node uses, often in a single mode, so they can't tell you which collections or modes exist. To know themes and brands, read the full collections with their modes: a connector's variables tool, the REST variables endpoint (Enterprise only, see above) or an exported variables file.
- **Aliases are in the raw values.** To see whether a semantic variable points at a primitive, read its raw value for each mode (in the plugin API, `valuesByMode`, where a link appears as a `VARIABLE_ALIAS`), not its resolved value: resolved values always look like plain colors or numbers. Never report tokens as unlinked from a resolved read.
- **Verify before flagging.** Before telling the designer something is wrong in Figma (unlinked tokens, missing modes, unbound values), confirm it with a read that can actually show it. If no such read is possible, say "couldn't check", not "looks wrong".
- **Private assets** (names starting with `_` or `.`) are skipped (`figma-hygiene.md`).
- **Connector limits.** Read-only connector tools often can't see the whole file. If your read method doesn't return every page, or returns only one node's variables, a partial page list or a single mode is not the file's structure.
- **Read-only scripts are reads.** Use read-only tools where they give the full picture. Where they don't, `design-spec-plan`, `design-spec-authoring`, `design-spec-preview` and `design-spec-audit` may run a plugin-script tool (for example the official connector's script tool, following Figma's own skill for it) without asking, as long as the code only reads: no creating, editing, renaming, moving or deleting anything. Say once that you did, for example "The connector only showed `Cover`, so I listed the pages and variable modes with a read-only script." If no such tool is available, ask the designer to name the pages and the variable modes (for example Light and Dark), or use the REST API or an export.
- **Any change is a write.** A script that changes anything in the file, however small, follows "Writing to Figma" below: offer first, act only on approval.
- **If something can't be read** (pages, collections, modes), say what and why, and ask the designer rather than assume. Never default a theme count, page list or component list from a failed or partial read.

---

## First-connection check

On first connecting, confirm the skill can read everything the specs will depend on:

- **Figma:** the file, the relevant nodes, variables, the Ready for dev status of components and sections, and the file `version` or `lastModified` where it can be read ("File version unreadable" below).
- **Documentation platform:** read and write inside the agreed root, page versions, and page properties.

If any of these can't be read, say so before writing anything that depends on it.

---

## Writing to Figma

The authoring skill reads Figma freely (including with read-only scripts, see "Reading a Figma file"), but writes to it only after an **explicit offer and the designer's approval**. Offer, don't act, when:

- A visual property is hard-coded instead of bound to a variable: "Want me to bind this to the right variable in Figma?"
- The alignment map recommends renames or moves: "Want me to apply these renames in Figma?"
- Tokens were defined in `spec-only` mode and the project is moving to Figma: "Want me to create these as Figma variables?"
- The designer asks for Figma comps of a spec-only set: "Want me to generate comps for these specs in Figma?" (`spec-only-mode.md`, §9).
- Figma annotations have been written into a spec: "Want me to remove these annotations from Figma now that the spec holds them?"

`design-spec-preview` and `design-spec-audit` never write to Figma.

---

## When a connection is unavailable

"Couldn't check" is never "checked".

**Figma unreachable (`figma` mode):**

- Don't invent values or recall them from memory.
- Write the behavior and structure sections; keep the node references already provided.
- Don't update `figma-version`.
- **File version unreadable** (Figma otherwise reachable): the REST API returns the file `version` and `lastModified` at the root of the file response (`GET /v1/files/<file key>?depth=1`, with a personal access token in the environment; any plan, no Enterprise needed). If your read method doesn't return them, `figma-version` can be unreadable even when everything else works. That's not a blocker: leave the field out, say once at the checkpoint that staleness checks won't run for those specs until it's recorded, and never ask the designer to fix access just for it. This bullet is the one owner of `figma-version` readability.
- Tell the designer once, in a batched message, what was deferred.
- Never switch to `spec-only` mode because Figma is unreachable.
- Resume the deferred parts when access returns.

**Documentation platform unreachable:**

- The local markdown working copy stays valid, but nothing is pushed.
- Tell the designer which specs are unsynced.
- On reconnect, re-read each page's version and merge rather than overwrite.

**Asset location unreachable (scene components):** keep the pointers as recorded and say which assets couldn't be checked.
