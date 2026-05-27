# Spreadsheet Workbench

The Spreadsheet workbench embeds a live, formula-capable spreadsheet directly
inside a FreeCAD document. Its primary purpose is not data entry — it is
**parametric model control**: one spreadsheet can drive dozens of dimensions
across multiple sketches, parts, and assemblies so that changing a single cell
ripples through the entire design.

---

## What the Spreadsheet workbench is for

### The parametric master-model pattern

The most powerful use of the Spreadsheet workbench is the *master-model
pattern*:

1. Create a spreadsheet with named cells (aliases) for every key dimension of
   your design: `Length = 200`, `Width = 50`, `WallThickness = 3`,
   `BoltHoleDia = 6.35`.
2. In every sketch and part feature, replace hard-coded numbers with
   spreadsheet references: instead of typing `200` in a Pad length, type
   `Spreadsheet.Length`.
3. To resize the whole design, edit one spreadsheet and recompute.

Without a spreadsheet, changing a design's proportions means hunting through
dozens of constraints and feature parameters. With a spreadsheet, it is one
cell change.

### Secondary uses

- **Bills of materials** — tabulate part counts, weights, costs.
- **Calculation scratch-pads** — perform multi-step engineering calculations
  whose result feeds into a dimension.
- **Configuration variants** — switch between design variants by changing a
  single "config" value that controls many downstream aliases via IF formulas.

---

## Key concepts

### Cells and cell addresses

Cells are addressed by column letter and row number: `A1`, `B3`, `Z100`.
Columns go A–Z then AA–AZ, BA–BZ, …

### Content types

A cell can contain:

| Content | How to enter | Example |
|---------|-------------|---------|
| Number | Type directly | `42` |
| Text | Type directly (no `=`) | `Length` |
| Formula | Prefix with `=` | `=A1 * 2.5` |
| Unit quantity | Number + unit | `200 mm` or `=200 mm` |

### Formulas and the expression engine

The `=` prefix activates FreeCAD's expression engine. Formulas can reference
other cells, use mathematical functions, and reference properties of any
FreeCAD object in the document. See
[Formulas and Expressions](tools/formulas-and-expressions.md) for the full
reference.

### Aliases

Every cell can have an **alias** — a human-readable name (`WallThickness`)
assigned in addition to its grid address (`C4`). Other tools reference the
alias rather than the address: `Spreadsheet.WallThickness` is far more
readable than `Spreadsheet.C4` and does not break when rows are inserted.

See [Aliases](tools/aliases.md) for details.

### Bidirectional linking

Spreadsheets can both **read** model properties (display a part's computed
volume in a cell) and **write** model properties (drive a Pad length from a
cell). The dependency direction matters: FreeCAD enforces that there are no
circular references.

---

## Relationship to other workbenches

| Workbench | Relationship |
|-----------|-------------|
| **Sketcher** | Sketch constraint values (length, radius, angle) can be driven by spreadsheet cell references, creating fully parametric sketches. |
| **Part Design** | Pad, Pocket, Hole depths, pattern counts, etc. can all be driven by spreadsheet aliases. |
| **Part** | Same as Part Design — any Part feature parameter can reference a spreadsheet cell. |
| **FEM** | Material properties and load magnitudes can be read from spreadsheet cells. |

See the [Tools index](tools/index.md) for a complete reference.
