# Spec: Decoration ↔ Material Linkage

**Status:** Draft — needs review with Colin before building  
**Branch:** TBD (new feature branch off feature/insert-sizes or main)

---

## Overview

Each decoration card gets an "Apply To" field where the designer selects which material row(s) the decoration applies to. The material table gets a read-only "Decoration" column that auto-populates based on those selections. This mirrors the insert sizes → binding type relationship but in the opposite direction: the decoration card drives the linkage, and the material row reflects it.

---

## Decoration Cards — "Apply To" Field

Each decoration card (Deboss, Laser-Cut/Engrave, Color Print, etc.) gets an **Apply To** dropdown or multi-select. Options are drawn dynamically from the current material rows, identified by row number and material name:

- Example options: `1 — Timber (Rockboard)`, `2 — Majilite Ovation Teal (Leather)`, `3 — ...`
- A decoration can apply to one or multiple materials (multi-select)
- If no material rows exist yet, the field is disabled with a hint ("add a material first")
- If a material row is removed, any decoration referencing it updates automatically (removes that option from the selection)

---

## Material Table — Read-Only "Decoration" Column

A new column added to both the exterior and interior material tables. It is not editable — it reflects which decoration cards have selected this row in their Apply To field.

- Example value: `Deboss 1, Laser-Cut 1`
- If no decorations reference this row: blank (or subtle "none" placeholder)
- Updates live as decoration Apply To selections change

---

## Comparison to Insert Sizes → Binding Linkage

| | Insert Sizes → Binding | Decoration → Material |
|---|---|---|
| **Who drives the link** | Insert row (selects binding type) | Decoration card (selects material) |
| **Who reflects it** | Binding card (read-only "Applies To") | Material row (read-only "Decoration" column) |
| **Auto-create behavior** | Selecting a binding type creates the binding card | No auto-create — decoration cards are added manually |
| **Orphan behavior** | Binding card turns red if no insert rows point to it | TBD — see open questions |

---

## Future Potential

This linkage is the foundation for:

- **Material-specific validation** — certain decorations require additional information depending on what they're applied to (e.g. wood engraving has different requirements than leather debossing)
- **Conditional fields** — decoration card fields could expand or change based on the selected material type
- **Production pathway routing** — knowing decoration + material combination enables downstream automation (vendor selection, process flags, etc.)

---

## Note on Pattern Direction

The user noted this could inform how the insert sizes pattern works going forward — potentially moving to a model where the **binding card** has the "Apply To" for insert rows (rather than the insert row selecting the binding type). This would make both systems directionally consistent (the "child" card always points to the "parent" row). Not proposed for immediate change — flagging for future consideration.

---

## Open Questions (confirm with Colin)

- Can a single decoration apply to multiple materials? (e.g. a deboss that spans both an exterior and a panel)
- Can a single material row have multiple decorations? (almost certainly yes)
- How should material rows be identified in the Apply To dropdown — by row number, by material name, or both? What if two rows have the same material?
- If a decoration has no material selected, is that a validation error (hard stop) or a soft warning?
- If a material row is removed that a decoration references, should the decoration show a warning similar to the binding card orphan behavior?
- Should Color Print and Other decoration types be exempt from requiring a material linkage, since they may not map cleanly to a single material?

---

*Created: 2026-06-11*
