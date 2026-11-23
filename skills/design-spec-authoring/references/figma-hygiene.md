# Figma Hygiene

Shared rules for the design-spec family, for **`figma` mode only**. These are recommendations for the designer's Figma file, not requirements. They never block authoring or previewing: work continues with the file as it is, and anything that doesn't follow them is mentioned briefly (authoring) or left to the audit. The one hard rule is for the agent, not the file: no writes to Figma without the designer's approval (last item).

---

## Contents

- Naming and sections
- Ready for dev
- Annotations are not a source
- Bind to variables
- Slots hold structure, not screen data
- Placeholder or real content
- Expose nested instances a screen changes
- Section order
- Private assets
- Writes to Figma

---

- **Naming and sections** follow "Matching names in Figma" in `spec-types.md`: type prefix in the component name and the section, slash convention, 1:1 with spec names.
- **Ready for dev:** recommend marking stable components and screens. Unmarked designs are treated as in flux (`source-of-truth.md`, "Ready for dev and buildable").
- **Annotations are not a source.** Behavior, states and notes belong in specs, not Figma annotations; coding agents read only visuals from Figma. Existing annotations are a useful starting point when first authoring a spec. Once their content is written into the spec, offer to remove them from Figma (only with approval), or ask the designer to, so the two can't drift.
- **Bind to variables:** visual properties in Figma SHOULD be bound to variables, not hard-coded.
- **Slots hold structure, not screen data:** don't type text or swap content directly inside a component's slot on a screen. Once a screen edits a slot's contents, that screen keeps its own copy and stops receiving changes to the main component. Put the data in a small nested component with text, boolean or instance-swap properties (for example a card's content component, or `ui/metric`), place it in the slot, and set those properties on each screen. Figma also can't bind a parent component's properties to layers inside a nested slot, which is another reason to give the content its own properties.
- **Placeholder or real content:** ui components show only placeholder content in angle brackets (`<Title>`, `<Label>`), because they are generic and get their content where they're used. Feature and scene components are flexible: placeholders are fine anywhere in them, main component or parts. Where content is the same on every screen (a header title, an input's placeholder text), prefer setting it in the feature or scene component over overriding it on each screen. Screens show realistic content (placeholder data, never real customer data), so they read as the product does.
- **Expose nested instances a screen changes:** when a screen needs to change a component nested inside another (the header inside a chat shell), expose that nested instance on the parent so its properties appear on the parent's instance. Set them there rather than editing the nested layers, so the change stays a property and the screen keeps receiving updates from the main component.
- **Section order:** arrange the sections within a page in the same order the specs list in (by default alphabetical by spec name), so a section is easy to find when comparing Figma with the specs. The `ui`, `feature` and `scene` sections themselves keep the order in `spec-types.md`.
- **Private assets:** styles, variables, components and sections whose names start with `_` or `.` (for example `_presentation/mockup-shadow`) are presentation or working assets. Figma doesn't publish them to the library, and the design-spec skills ignore them: they never become tokens, specs or alignment-map entries. Recommend this prefix for anything in the file that isn't part of the product (mockup shadows, device frames, annotation helpers).
- **Writes to Figma** (renames, fixes, pushing tokens or components) happen only with the designer's explicit approval (`connections.md`, "Writing to Figma").
