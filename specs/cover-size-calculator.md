# Spec: Auto-Calculate Cover Size + Ring Binder Redesign

**Status:** Draft — needs review with Colin before building  
**Branch:** feature/insert-sizes (or new branch)

---

## Feature 1: Auto-Calculate Cover Size

### Overview
A button next to the Cover Size field that derives the cover size from the insert sizes table and active binding types. Clicking it populates the Cover Size field automatically.

### Calculation Logic

1. Read all insert size rows. Find the **largest width** and the **largest height** across all rows.
2. Start with a base addition of **+0.5"** to both width and height.
3. Apply binding type overrides:

| Binding Type Present | Override |
|---|---|
| Menu Cord | Height gets **+1"** instead of +0.5" |
| Ring Binder (¾" ring selected) | Width gets **+1"** instead of +0.5" |
| All other binding types | No override — use +0.5" defaults |

4. Output format: `W" × H"` written into the Cover Size field.

### Open Questions (confirm with Colin)
- If both Menu Cord **and** a ¾" Ring Binder are active, do both overrides apply? (width +1", height +1")
- Are there any other binding types that affect cover size beyond Menu Cord and Ring Binder?
- If insert width/height fields are blank, should the button show a toast ("please fill in your insert sizes") and do nothing?
- Should the button overwrite an existing cover size value without warning, or prompt first?

---

## Feature 2: Ring Binder Card Redesign

### Overview
Replace the current free-text "Size / Color" field on the Ring Binder binding card with structured selections. The selected ring size feeds directly into the cover size calculation.

### Ring Options (from reference sheet)

**2 Ring**
| Part # | Specs | Max Inserts |
|---|---|---|
| 0916-3102 | ½" ring, 3¼" apart | ~30 |

**3 Ring**
| Part # | Specs | Max Inserts |
|---|---|---|
| STR212-3-13 | 15mm, short, 8½" insert | ~25 |
| STR277-3-13 | 15mm, long, 11" insert | ~25 |
| IM85N | ½", short, 8½" insert | ~30 |
| 0905-7101 | ½", long, 11" insert | ~30 |
| IN87N | ¾", short, 8½" insert | ~40 |
| 0975-7101 | ¾", long, 11" insert — *nickel not stocked in-house* | ~40 |
| Custom | Free text input | — |

### Color Options
- Nickel
- Gold
- Black

*(Multi-select — a job may need multiple colors)*

### Card Fields (proposed)
1. **Ring selection** — grouped by 2 Ring / 3 Ring, shows part number + specs, includes Custom option with text input
2. **Color** — Nickel / Gold / Black checkboxes
3. The note "*NEED NUMBER OF PAGES BEFORE ORDERING*" displayed as a warning on the card

### Open Questions (confirm with Colin)
- Does this ring list represent the full current inventory, or does it change often? If it changes, a free-text field may be more practical long-term.
- Should the "nickel not stocked in-house" warning appear visually on the card when 0975-7101 + Nickel are both selected?
- Is the Custom option needed, or does the structured list cover all cases?
- Should the card display the max insert count as a reference once a ring is selected?

---

*Created: 2026-06-11*
