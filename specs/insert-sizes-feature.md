# Spec: Insert Sizes, Binding Type Linkage & Related Changes

**Status:** Built — retroactive documentation for Colin's review  
**Branch:** feature/insert-sizes

---

## Overview

This feature replaced the old single-row "size specs" approach with a dynamic insert sizes table. Each row in the table links directly to a binding type card, replacing what was previously a manual, disconnected workflow.

---

## Insert Sizes Table

### Columns
| Column | Description |
|---|---|
| # | Auto-numbered row index |
| Width | Numeric input (decimal allowed), displays "in." suffix visually |
| × | Decorative separator |
| Height | Numeric input (decimal allowed), displays "in." suffix visually |
| Binding Type | Single-select dropdown — Corners, Tabs, Menu Cord, Screwposts, Ring Binder, Clips, Bands, Other |
| Count | Free text (number of inserts for this size) |
| × | Remove row button |

### Behavior
- Page loads with one insert row pre-populated
- "+ Add insert size" button adds additional rows
- Width and height fields are restricted to numbers and a single decimal point
- Removing the last row clears it instead of deleting it (always at least one row)
- The count displays in the "applies to" label as "(20 inserts)"

---

## Binding Type Linkage

### How it works
Selecting a Binding Type in an insert row automatically creates the corresponding binding card in the Binding Type section — no need to manually add it.

The manual "add binding type" buttons (+ Corners, + Tabs, etc.) are hidden. Binding cards are only created through the insert sizes table.

### Applies To label
Each binding card shows an "Applies to" summary of the insert rows linked to it:

- Example: `Insert Size(s): 4.25 × 11 (20 inserts), 4.25 × 8.5 (10 inserts)`
- If no insert rows point to a binding card: red border + "No insert sizes assigned" warning
- If a binding card becomes unlinked (insert row removed or binding type changed):
  - Card has values filled in → red border, stays (user must resolve manually)
  - Card is empty → auto-deleted, add button restored

### Keyboard support
Selecting a binding type via keyboard (arrow keys + Enter) triggers the same auto-add behavior as clicking.

---

## Customizations Section

Moved from the old "Size Specs" area into its own dedicated section between Insert Sizes and Material:

- **Sailcloth Reveal** — size input + Edge multi-select (Exposed, Closed, Flush). Hidden until "+ Add sailcloth reveal" is clicked.
- **Custom Board Type** — free text input. Hidden until "+ Add Custom Board Type" is clicked.

---

## Material Table Changes

- All type dropdowns (exterior, interior) now show "Select Type" as the default placeholder
- Empty row detection accounts for the placeholder (a row showing only "Select Type" is treated as empty for validation purposes)
- "Add exterior material" and "Add interior material" buttons match the Insert Sizes add button style (full-width, light gray)

---

## Insert Type — Removed

An "Insert Type" column (Inserts / Pages) was built and subsequently removed. All inserts are referred to simply as "inserts." The state format retains backward compatibility — old PDFs with a `type` field in their embedded state will restore without error, the field is just ignored.

---

## State & PDF Compatibility

Insert sizes are saved in the PDF's embedded state block as:

```json
"inserts": [
  { "w": "4.25", "h": "11", "count": "20", "binding": "Corners" },
  { "w": "4.25", "h": "8.5", "count": "10", "binding": "Tabs" }
]
```

Dragging a saved PDF back onto the tool restores all insert rows and reconstructs the binding cards.

---

## Open Questions for Colin
- Are there binding types that a job might have without a corresponding insert size (i.e., cases where the manual add buttons being hidden is a problem)?
- Is "inserts" always the right unit, or are there jobs where "pages" is more accurate? (Insert type was removed but could be restored if needed.)
- Does the count field need any validation (must be a whole number, must be within ring binder capacity, etc.)?

---

*Created: 2026-06-11*
