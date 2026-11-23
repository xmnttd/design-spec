# Figma Checks

How the audit reads Figma and compares it with the specs. Applies when `visual-source: figma`, and to any spec with `figma-nodes`. The checks themselves are in `checks.md` (FIG-1 to FIG-11, MTN-7, and REF-4, LIB-5 and LIB-6 where they compare Figma frames).

---

## 1. Access

- **Read-only.** The audit never writes to Figma. Before running a plugin script, load Figma's own skill for its script tool. Read-only scripts: `../design-spec-authoring/references/connections.md`, "Read-only scripts are reads".
- **Only these methods:** `connections.md`, "Only these methods".
- **Code Connect:** `connections.md`, "Code Connect".
- **Files and methods** come from `figma-access` in `intent.md`. Scope library lookups to the file's `library-key`.
- **Private assets** (names starting with `_` or `.`) are ignored (`../design-spec-authoring/references/figma-hygiene.md`).

---

## 2. What to read

Only what the specs point to:

- Each spec's `figma-nodes`: the node, its variants and component properties, the instances it contains, and any annotations or description on it.
- For `tokens.md` (and `motion.tokens.md`, if it declares a mirror): the named variable collections, with modes and raw values, plus the list of all local collections (for FIG-11).
- Ready for dev and the file version, where readable. Ready for dev comes from the REST API (`devStatus`); the file version from REST or any read method that returns it (`../design-spec-authoring/references/connections.md`, "File version unreadable"). When they can't be read they're setup limits, reported once, never findings (`reporting.md`).

Never browse pages or nodes that no spec references, except to resolve a component nested in a referenced node.

---

## 3. Drift: the structural compare

Specs hold no visuals in `figma` mode, so drift is structural. For each referenced node, compare:

| Spec | Figma | Check |
|---|---|---|
| The node in `figma-nodes` | The node exists, and is a main component, set, section or frame | FIG-1, FIG-2 |
| States table, Variants table | Variant property values (for example `state=hover`) or boolean properties (`disabled=true`), matched by exact name. Kit variants a library-backed spec doesn't use are skipped | FIG-3, FIG-4 |
| Properties table | Component properties: names, types, variant options | FIG-5 |
| `status` | Ready for dev (when readable) | FIG-6 |
| Components a screen cites | Instances in the screen's frames, by main component | REF-4 |
| Library-backed specs, "Used in this product" | Kit instances in the screen's frames, and the options they use | LIB-5, LIB-6 |
| Spec content | Annotations and descriptions on the node | FIG-7 |
| Spec and section names | Figma component and section names | FIG-8 |
| `motion.tokens` (when mirrored) | The mirroring collection's variable names and values | MTN-7 |

Bound and hard-coded values (FIG-9) and edited slot content (FIG-10) are read from the same nodes. For FIG-10, compare each screen instance's slot children with its main component's slot (the number, names and types of children): text typed or layers swapped directly in the slot show up as a difference. A read-only script can do this.

**When it runs:** on changed specs and their neighbors, and on every full sweep (`run-process.md`, step 2).

**The version shortcut:** if the file version is readable and equals the version in the last run log, Figma hasn't changed: skip the compare for unchanged specs. A skipped compare counts as "not run", so its findings are carried over unchanged (`register.md`, §3). If the version is readable and has changed, compare every referenced node, not only changed specs. A spec's own `figma-version` that is older than the file is not a finding by itself; it only triggers the compare for that spec.

---

## 4. Reading efficiently

- Read metadata first, then only the properties needed for the compare. Send reads for several nodes as one parallel batch.
- A component set returns every variant in one read.
- Screen state frames: read which components and states they hold, not a full rebuild.
- If a read is rate-limited or fails part-way, record those specs' Figma checks as "couldn't check" for this run and continue.

---

## 5. When Figma can't be read

- **Unreachable:** every Figma check for this run is "couldn't check". Findings from earlier runs stay as they were (`register.md`, "Reconciling"). Say so in the report.
- **A node can't be read** (no access, deleted or moved): if the file is reachable and the node ID doesn't exist, it's FIG-1. If access failed, it's "couldn't check".
- Never fill gaps from memory or from names alone.
