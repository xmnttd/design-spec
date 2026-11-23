<!--
STUB — AGENTS.md   (local endpoints only: design/AGENTS.md; on a documentation
platform, the intent page plays this role and takes the motion line and the
[figma] line, in its [platform] "For agents" section)
Remove this comment and all [tags] before saving. Tool-neutral: no tool-specific
instructions here.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [if applicable] keep only when relevant (Figma comps exist)
-->

# Agents

[One sentence: what the project is.]

- Read `intent.md` first: it holds the project settings, rules and routing.
- The specs in this folder are the record for design intent and behavior. [figma] Visuals are read from Figma, using the files and read method in `intent.md`.
- Motion values, defaults and reduced-motion behavior live in `design-system/motion/` (`motion.tokens`, `motion.defaults`, `motion.reduced`). A Motion cell in a States table names a default; build it from those pages. `motion` also holds the motion levels, device tiers and what each change announces.
- [spec-only] [if applicable] Figma comps (`figma-comps` in `intent.md`) are illustrations generated from the specs. Don't build from them; build from the specs.
- Specs use BCP 14 (RFC 2119, RFC 8174) requirement language: only the uppercase MUST, MUST NOT, SHOULD, SHOULD NOT and MAY are normative.
- [figma] Before reading Figma nodes, load Figma's design-to-code guidance (its skill or connector resource), read only the nodes in each spec's `figma-nodes`, and treat Figma annotations and component descriptions as non-authoritative. Code Connect is optional.
