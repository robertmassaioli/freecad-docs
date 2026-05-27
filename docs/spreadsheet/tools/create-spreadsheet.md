# Create Spreadsheet

> **In one sentence:** Add a new, empty spreadsheet object to the active
> FreeCAD document — the starting point for every parametric model driven
> by a spreadsheet.

---

## Overview

**Workbench:** Spreadsheet · **Menu:** Spreadsheet → New Spreadsheet  
**Toolbar:** Spreadsheet · **Shortcut:** none

Create Spreadsheet inserts a `Spreadsheet::Sheet` object into the model tree.
The spreadsheet opens automatically as a tab in the FreeCAD view area, showing
a familiar grid of rows and columns.

---

## Intuition

A FreeCAD spreadsheet is not a separate file — it is a full model object,
saved inside the `.FCStd` file alongside your geometry. This means:

- The spreadsheet travels with the model; no external `.xlsx` or `.csv` to track.
- It participates in the document's expression engine: cells can reference
  model properties and model features can reference cell values.
- Multiple spreadsheets can coexist in one document (one per sub-assembly,
  for example).

The typical workflow is: create one spreadsheet per document → add alias-named
rows for every key dimension → reference those aliases everywhere in the model
→ tune the design by editing the spreadsheet.

---

## When to use it

- Starting a new parametric model: create the spreadsheet before adding any
  geometry so dimensions can be defined upfront.
- Adding parametric control to an existing model: create the spreadsheet,
  populate it with the current hard-coded values, set aliases, then replace
  the hard-coded numbers with spreadsheet references.

## When NOT to use it

- **Simple models with one or two dimensions** — direct formula entry in the
  constraint dialog is simpler. A spreadsheet adds value when multiple features
  share dimensions or when the design has many variants.

---

## Step-by-step walkthrough

1. **Switch to the Spreadsheet workbench.**
2. **Spreadsheet → New Spreadsheet** (or click the toolbar button).
3. A `Spreadsheet` object appears in the model tree. The spreadsheet editor
   opens in a new view tab.
4. Click a cell (e.g. `A1`) and type a label: `Length`.
5. Click `B1` and type the value: `200 mm`.
6. Right-click `B1` → **Properties** → set an alias: `Length`.
7. In a sketch constraint, type `=Spreadsheet.Length` to link the constraint
   to this cell.

---

## The spreadsheet editor

The spreadsheet editor is a grid view with:

| Element | Description |
|---------|-------------|
| **Cell address box** | Shows the address of the selected cell (e.g. `B1`). Editable — type an address and press Enter to jump there. |
| **Content bar** | Shows and edits the raw content of the selected cell (formula or value). |
| **Column headers** | A, B, C … Click to select a whole column. |
| **Row headers** | 1, 2, 3 … Click to select a whole row. |
| **Green cell border** | Indicates a cell has an alias set. |

### Keyboard shortcuts inside the editor

| Key | Action |
|-----|--------|
| Enter / Tab | Confirm and move to the next cell (down / right) |
| Escape | Cancel editing |
| Delete | Clear selected cell(s) |
| Ctrl+C / Ctrl+V | Copy / Paste cells |
| Ctrl+Z | Undo |

---

## Renaming and multiple spreadsheets

A new spreadsheet is named `Spreadsheet` by default. To rename it:

1. Right-click it in the model tree → **Rename**.
2. Type a new internal name (e.g. `Params`).

References to cells use the internal name: `Params.Length`. If you rename the
spreadsheet, all existing references update automatically.

To have multiple spreadsheets (e.g. one for dimensions, one for material
properties), simply run **New Spreadsheet** again.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Create a spreadsheet
sheet = doc.addObject("Spreadsheet::Sheet", "Params")

# Set cell values
sheet.set("A1", "Length")
sheet.set("B1", "200 mm")

sheet.set("A2", "Width")
sheet.set("B2", "50 mm")

sheet.set("A3", "Height")
sheet.set("B3", "10 mm")

# Set aliases
sheet.setAlias("B1", "Length")
sheet.setAlias("B2", "Width")
sheet.setAlias("B3", "Height")

doc.recompute()

# Read cell values back
print(sheet.get("B1"))          # 200.0 (as float, unit stripped)
print(sheet.getAlias("B1"))     # "Length"

# Use alias in a formula
sheet.set("B4", "=B1 + B2")    # = 250 mm
doc.recompute()
print(sheet.get("B4"))          # 250.0
```

---

## See also

- [Formulas and Expressions](formulas-and-expressions.md) — formula syntax reference
- [Aliases](aliases.md) — naming cells for external references
- [Model Integration](model-integration.md) — linking spreadsheet values to model dimensions
- [Import / Export](import-export.md) — load an existing CSV instead of starting empty
