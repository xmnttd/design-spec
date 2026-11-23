---
title: icons
type: foundation
status: draft
last-updated: YYYY-MM-DD
figma-nodes:                      # [figma]
  file: "[Figma file URL]"
  nodes: ["[icon library frame node ID]"]
figma-version: "[version or lastModified]"   # [figma]
assets:                           # [if applicable]
  - name: "[icon set name]"
    format: "[e.g. svg]"
    location: "[repository path or URL]"
    version: "[version]"
---

<!--
STUB — ICONS
Remove this comment, all [tags] and all unused blocks before saving.

Tags:  [figma] keep only in figma mode · [spec-only] keep only in spec-only mode
       [if applicable] keep only when relevant

This file owns everything about icons except the artwork:
source, sizes, color rule, implementation rules, assignments and approved usage.
Artwork lives in Figma [figma], or in a named library / asset files [spec-only].
Never inline SVG.
Front matter: `assets` lists icon files that don't come from a named
installable library, as pointers only.
-->

**Role:** [One sentence: icon style and source.]

---

## Source

[figma] Artwork lives in the icon library frame referenced above.

[spec-only]
- **Library:** [name], or **asset files:** [location]
- **Format:** [e.g. SVG files, icon font]
- **Version:** [version]
- **License:** [license]
- **Install:** [install method, if a package]

**Style:** [e.g. outline only; filled reserved for selected states.]

---

## Sizes

| Token | Use |
|---|---|
| `--size-icon-sm` | [Inline with text, compact UI] |
| `--size-icon-md` | [Default] |
| `--size-icon-lg` | [Empty states, feature callouts] |

---

## Color

[Rule, e.g. icons inherit color from their parent via `currentColor`; fill and stroke are never hard-coded.]

---

## Implementation rules

- Icons MUST come from the approved inventory below.
- Size MUST be set via a size token.
- Decorative icons MUST be hidden from assistive technology; icon-only controls MUST have an accessible name.
- [Other rule.]

---

## Assignments and approved usage

| Icon | Meaning | Use in | Don't use for |
|---|---|---|---|
| [icon-name] | [What it signals] | [spec names or contexts] | [Common misuse] |
