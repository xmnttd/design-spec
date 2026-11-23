# Endpoints

Where a spec set lives, and how specs are written there. Two kinds of endpoint:

- **Local markdown:** files in a folder, matching the tree in `spec-types.md`.
- **Documentation platform:** any wiki or knowledge-base tool (for example Confluence or Notion). These rules are written generically; platforms are named only as examples.

How to reach an endpoint (connections, credentials, unavailable access) is in `connections.md`.

---

## Contents

- One record, never two
- Outside code repositories
- Local markdown
- Documentation platforms

---

## One record, never two

Wherever specs live, one system is the record. Anywhere else, a spec appears only as a reference or link to the record, never as a maintained copy.

- **Local markdown endpoint:** the files are the record.
- **Documentation platform endpoint:** the platform pages are the record. A spec may be drafted as a temporary local markdown working copy and then pushed. The working copy lives outside any code repository and outside the platform's page tree (for example in the session's working folder); once pushed, the local file is discarded, not kept in sync. To edit again, re-read the page (pulling a fresh working copy if needed). Never keep a local copy around and push it over the page.

---

## Outside code repositories

The spec set lives **outside engineering's code repositories**, in its own folder or on a documentation platform. Engineers and their coding agents read specs from the endpoint, by path or link. Specs are never copied into, committed to or maintained inside a code repository, and code never goes into the spec set.

- **Local markdown:** the `design/` folder sits outside any code repository. If the folder the designer gives is inside one (it or a parent folder contains `.git`), say so and recommend a location outside it before scaffolding.
- **Generated output** such as the preview stays outside both the spec set and code repositories.

---

## Local markdown

- Follow the folder structure in `spec-types.md` exactly.
- One file per spec; filename = spec name + `.md`.
- `design/AGENTS.md` (from `stubs/agents.md`) gives tool-neutral instructions to any agent reading the folder.
- Version check before writing: the file's modified time or content hash.

---

## Documentation platforms

### Structure

- The tree in `spec-types.md` maps to the platform hierarchy under the **root** agreed at scoping. Every write stays inside that root.
- **Folders** become real folders where the platform supports them (they usually list their contents when opened). Where it doesn't, use a **parent page with a child-page listing**.
- A folder can't hold content, so `intent` is always a page at the top of the tree. It also plays the role `AGENTS.md` plays locally.
- `report/` pages live in their own folder or parent page, clearly separate from specs.

### Page titles

- **Page title = spec name** (filename minus `.md`): `ui.modal.bundle`, `screen.dashboard`, `intent`, `tokens`. Keeping the prefix keeps search and sorting grouped by type.
- The front matter `title` is the spec name, so it is the page title. Specs have no written H1: the platform renders the title as the page heading.
- **Uniqueness:** many platforms require unique titles within a space. Spec names are unique within a project. If several projects share one space and titles would collide, add a short project key to every page title consistently (for example `acme · ui.button`), applied by the mapping. The front matter `title`, spec names and references stay unprefixed; only the displayed page title carries the key.
- **Plain-text titles, no auto-links:** spec names contain dots, so some platforms (Notion, for example) turn part of a name into a web link (`feature.app` in `feature.app-header`). Write titles as plain text with no link formatting, and after creating or renaming a page, check that the title came back as plain text; if the platform added a link, remove it. In page content, write spec names in code format (`ui.button`), which platforms don't auto-link.
- **Disallowed characters:** check what the platform blocks in titles before creating pages. Kebab-case spec names avoid the usual problem characters (such as `: / \ | # ; ^ @ [ ] { } < >`), but verify against the connector in use.

### Front matter

Markdown content (headings, tables, lists, code blocks) generally carries over. The YAML front matter block generally does not: most platforms don't treat it as metadata and would show it as plain text.

- Translate front matter into the platform's **native page metadata** (page properties, database properties or an equivalent properties block), in the same fixed field order as `spec-types.md`.
- If the platform records the last-edited time natively, map `last-updated` to it rather than keeping a separate property; the same applies to any other field the platform already owns.
- Nested fields (`figma-nodes`, `assets`) become a compact properties table or a properties block, keeping every subfield.
- Verify the translation against the connector in use on first push, then use it consistently.
- When pulling a page back to a local working copy, convert the properties back into valid YAML front matter.

### References

- References between specs become **links** to the referenced page where the platform supports them, with the spec name as the link text.
- When a page is renamed, update links as part of the rename (see `lifecycle.md`).

### Versions

- Read the page's version (a version number or last-edited timestamp) when reading the page, and check it again immediately before writing. If it changed, re-read and merge.
- After writing, re-read the page to confirm the content and properties landed correctly.

### Formatting cautions

- Prefer simple structures: headings, tables, lists and code blocks translate most reliably.
- Avoid relying on HTML comments, footnotes or nested tables; many platforms drop or mangle them.
- Stub comments and tags are removed before pushing (see "Stub cleanup" in `best-practices.md`).
