# Choosing Types

Authoring only. How to recommend a spec type, detect a bundle and bring an existing Figma file's names into line, in conversation with the designer. The types, bundle rules and naming rules themselves are in `spec-types.md`.

---

## Contents

1. Recommending the type
2. Detecting a bundle
3. Aligning an existing Figma file

---

## 1. Recommending the type

Quick test (type definitions: `spec-types.md`, §2):

| Question | If yes |
|---|---|
| Could it appear unchanged on unrelated screens, with placeholder content? | ui-component |
| Is its content, copy or data shape specific to one product context? | feature-component |
| Is its main content an asset made outside Figma (3D, video, interactive animation)? | scene-component |
| Is it a whole screen? | screen |
| Is it a sequence of steps across screens? | flow |

If the model will use it in generated UI, it's still a ui or feature component; add the Generative use block.

Many designers are new to specs and won't know the difference between these types, so don't expect them to name one. Unless the designer has stated the type, **recommend it and confirm before writing**: give the recommended type, the spec name it implies, and a one-line reason drawn from the quick test. For example:

> "This looks like a **feature component** (`feature.panel-booking-summary`): it shows real booking data and copy and is only used in the booking flow. It builds on your generic panel (`ui.panel`). Shall I spec it that way?"

**Library-backed components.** When `components.source` in `intent.md` isn't `custom`, check whether the component comes from the library (or its Figma kit). If it does, recommend a library-backed ui spec, name the library component it's built on, and, in a set that started with the library, recommend the spec name that matches it (`spec-types.md`, §5, "Library-backed names"); if it's custom, recommend the full stub.

**Judge by contents, not by name.** A Figma component may already carry a prefixed name (`feature/panel-booking-summary`), a name without our convention (`panel-summary`), or a misleading one. Always check what the component actually contains (placeholder or real copy, data shape, nested components, where it's used) and recommend the type from that. If an existing prefix disagrees with the contents, say so.

**Recommend the Figma name in the same message.** Once the type is clear, if the Figma name doesn't match the convention (`spec-types.md`, "Matching names in Figma"), include the recommended Figma name in the same confirmation, for example: "Figma calls it `panel-summary`; to match the spec it would be `feature/panel-booking-summary`. Want me to rename it, or will you?" Renames happen only with approval. A declined rename is fine (node IDs are the binding) and is left for the audit to track.

If the designer has stated a type that the quick test contradicts, say so once with the reason, then follow their decision. The type determines the filename, the stub and the front matter, so this is a blocking question; batch it with any other blocking questions for the same spec.

---

## 2. Detecting a bundle

Whenever a component has sub-components, check before writing whether it's a bundle candidate (bundle rules: `spec-types.md`, §3), whether the designer pointed at the parent component alone or at a section containing the parent and its sub-components:

1. Identify the sub-components (in `figma` mode, the child components of the component set or section; in `spec-only` mode, from the designer's description).
2. Check whether any sub-component is used on its own or inside another component. In `figma` mode, look for instances outside the parent; if usage can't be read, say so rather than assuming.
3. Separate **parts** from **nested components**. Parts are the parent's own structural pieces (a modal's header, body, footer). A nested instance of a generic building block (a badge, button or icon button inside the modal) is not a part: it is a ui component in its own right and gets its own spec, even if it isn't used anywhere else yet. Early in a project, "not used elsewhere yet" doesn't mean "never will be".
4. Recommend: a bundle for the structural parts used only within the parent, with proposed part names (`ui.modal.header`, …); separate specs for anything used independently or generic enough to be reused. Give the reasoning in a line, including any part that isn't reused yet but likely will be.
5. **Always ask the designer to confirm**, even when the evidence looks clear; they know what's planned. Write nothing until they confirm.

---

## 3. Aligning an existing Figma file

`figma` mode. When a Figma file uses its own conventions or mixes component types, it is not renamed silently. Produce an alignment map in chat for the designer to approve, following "Matching names in Figma" in `spec-types.md`:

| Current name | Current location | Inferred type | Recommended name | Recommended section |
|---|---|---|---|---|
| `Btn Primary` | Page "Components" | ui-component | `ui/button` (variant `intent=primary`) | `ui` |
| `OrderSummary` | Page "Checkout" | feature-component | `feature/panel-order-summary` | `feature` |

The designer accepts all, some or none. Renames are applied by the designer, or in Figma only with explicit approval; renaming a main component does not break its instances. Names that stay different are fine; node IDs are the binding. Library kit components keep the kit's names and never appear in the map. Private assets (names starting with `_` or `.`) are skipped (`figma-hygiene.md`). Ongoing drift is monitored by the audit.
