# Model Integration

> **In one sentence:** Connect a spreadsheet to the rest of the FreeCAD model
> — driving part dimensions from cell values and reading model-computed
> properties back into the spreadsheet.

---

## Overview

**Workbench:** Spreadsheet (and all workbenches that accept expressions)  
**No dedicated toolbar button** — integration is done through the standard
expression system, available in any parameter field that shows a blue
expression icon.

---

## Intuition

A spreadsheet sitting in a document but disconnected from the geometry is just
a calculator. The power comes from wiring it to the model:

- **Spreadsheet → Model:** A cell value drives a Pad depth, a sketch constraint,
  a hole diameter. Change the cell; recompute; the geometry updates.
- **Model → Spreadsheet:** A cell formula reads back a computed model property
  (the volume of a solid, the length of an edge). The spreadsheet becomes a
  live bill-of-materials or results dashboard.

These two directions can coexist in the same document, but **not in a cycle**:
if cell A reads model property P, then P must not be driven by cell A
(or anything A drives). FreeCAD enforces this and reports a circular dependency
error if a cycle is detected.

---

## Direction 1: Spreadsheet drives model dimensions

### Step-by-step: driving a Sketch constraint

1. **Create the spreadsheet** and set up an alias (e.g. `B1 = 200 mm`,
   alias `BoxLength`).
2. **Open the sketch** in edit mode.
3. **Double-click the constraint** you want to drive (e.g. a Horizontal
   Distance constraint). The constraint dialog opens.
4. Click the **blue expression icon** (small `f(x)` button) next to the value.
5. In the formula bar, type `Spreadsheet.BoxLength` and press Enter.
6. The constraint value is now controlled by the spreadsheet. A small blue `f`
   marker appears on the constraint in the sketch.
7. Close the sketch. In the spreadsheet, change `B1` to `300 mm` and press
   **Ctrl+Shift+R** (or Model → Refresh / recompute). The sketch updates.

### Step-by-step: driving a Pad depth

1. Create a Pad from the sketch.
2. In the Pad dialog or the Data panel, click the expression icon next to
   **Length**.
3. Type `Spreadsheet.BoxHeight` (or whatever alias you have for the height).
4. Click OK.

### Parameter fields that accept expressions

Any parameter field with a blue expression icon (`f(x)`) accepts expressions.
Common examples:

| Workbench | Parameter |
|-----------|-----------|
| Sketcher | Any constraint value (distance, angle, radius) |
| Part Design | Pad/Pocket Length, Revolution Angle, Pattern Count, Fillet Radius |
| Part | Box Length/Width/Height, Cylinder Radius/Height, Extrude LengthFwd |
| FEM | Load magnitude, Constraint value |

---

## Direction 2: Model properties in spreadsheet cells

### Referencing a property in a formula

Use the syntax `<ObjectName>.<PropertyName>`:

```
= Box.Length          # Length property of an object named "Box"
= Pad.Length          # Depth of a Pad feature
= Sketch.ConstraintX  # Not directly; use named constraints
= Body.Shape.Volume   # Not via expression engine (use Python)
```

### Finding the correct property name

1. Select the object in the model tree.
2. Open the **Data** panel (View → Panels → Data).
3. The property names listed there are the ones the expression engine can read.
4. Common readable properties:

| Object type | Useful properties |
|-------------|------------------|
| `Part::Box` | `Length`, `Width`, `Height` |
| `Part::Cylinder` | `Radius`, `Height` |
| `PartDesign::Pad` | `Length`, `Length2` |
| `PartDesign::Fillet` | `Radius` |
| `Sketcher::SketchObject` | `Constraints.ConstraintName` (if named) |

### Example: reading back the Pad depth

```
# In spreadsheet cell C5:
= Pad.Length
```

If the Pad was driven by a different cell, this creates a circular dependency.
Only read back properties that are **not** themselves driven by the spreadsheet.

---

## The master-model pattern in practice

The canonical layout for a parametric part:

```
A              B               C (formula preview)
──────────────────────────────────────────────────
Length         200 mm          ← alias: Length
Width           50 mm          ← alias: Width
Height          10 mm          ← alias: Height
WallThickness    3 mm          ← alias: WallThickness
──────────────────────────────────────────────────
Computed values (read-only):
──────────────────────────────────────────────────
Cross area     = Length * Width            → 10000 mm²
Shell volume   = (Length*Width - (Length-2*WallThickness)*(Width-2*WallThickness)) * Height
```

Top section: **input parameters** — these drive the model.  
Bottom section: **derived quantities** — these read back computed results.

---

## Named constraints (for readable sketch references)

Sketch constraints can be named so they appear in the expression engine:

1. In sketch edit mode, double-click a constraint name in the Constraints panel
   (left side). A rename dialog appears.
2. Set a readable name (e.g. `HoleRadius`).
3. In other expressions (or in the spreadsheet), reference it as:
   `Sketch.Constraints.HoleRadius`

This is useful for reading a sketch's constraint value *back* into the
spreadsheet (model → spreadsheet direction).

---

## Common mistakes and pitfalls

!!! warning "Circular dependency"
    If cell A drives parameter P, do not also write a formula that reads P back
    into any cell that A depends on. FreeCAD detects cycles and reports an error
    on recompute.

!!! warning "Model does not update automatically"
    After changing spreadsheet values, press **Ctrl+Shift+R** or go to
    **Model → Refresh** to trigger a full recompute. FreeCAD does not always
    auto-recompute on cell change (depends on settings).

!!! warning "Expression not accepted in a dialog"
    Some older Part workbench dialogs (like the Extrude dialog) do not show
    the expression icon. In those cases, create the feature first, then bind
    the expression via the Data panel after the feature is created.

!!! warning "Property name capitalisation"
    FreeCAD property names are case-sensitive. `Box.length` does not work;
    `Box.Length` does. Check the exact name in the Data panel.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# --- Setup spreadsheet ---
sheet = doc.addObject("Spreadsheet::Sheet", "Spreadsheet")
sheet.set("A1", "Length"); sheet.set("B1", "200 mm"); sheet.setAlias("B1", "Length")
sheet.set("A2", "Width");  sheet.set("B2", "80 mm");  sheet.setAlias("B2", "Width")
sheet.set("A3", "Height"); sheet.set("B3", "30 mm");  sheet.setAlias("B3", "Height")

# --- Create a Part Box driven by the spreadsheet ---
box = doc.addObject("Part::Box", "Box")
# Bind expressions to the box properties
box.setExpression("Length", "Spreadsheet.Length")
box.setExpression("Width",  "Spreadsheet.Width")
box.setExpression("Height", "Spreadsheet.Height")

doc.recompute()
print(f"Box: {box.Length} × {box.Width} × {box.Height}")
# → Box: 200.0 mm × 80.0 mm × 30.0 mm

# --- Change a spreadsheet value and recompute ---
sheet.set("B1", "300 mm")
doc.recompute()
print(f"Box length after change: {box.Length}")
# → Box length after change: 300.0 mm

# --- Read a model property back into the spreadsheet ---
# (read-back direction — not the same cell/formula that drives it)
sheet.set("B5", "=Box.Width")   # reads the Width property
doc.recompute()
print(sheet.get("B5"))          # 80.0
```

---

## See also

- [Aliases](aliases.md) — naming cells so expressions are readable
- [Formulas and Expressions](formulas-and-expressions.md) — expression syntax reference
- [Create Spreadsheet](create-spreadsheet.md) — setting up the spreadsheet
