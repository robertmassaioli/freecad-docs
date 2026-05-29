# Cell Aliases and the Master-Model Pattern

> **In one sentence:** Giving spreadsheet cells human-readable names (aliases)
> and wiring them to model dimensions transforms a spreadsheet from a passive
> calculator into a live parameter panel — the *master-model pattern* that is
> the backbone of robust parametric design in FreeCAD.

---

## What is a cell alias?

A **cell alias** is a name you assign to a specific cell. Instead of
referencing that cell by its address (`Spreadsheet.B3`), any other object in
the document can reference it by name (`Spreadsheet.WallThickness`).

| Without alias | With alias |
|---------------|-----------|
| `Spreadsheet.B3` | `Spreadsheet.WallThickness` |
| Breaks if a row is inserted above row 3 | Travels with the cell — survives row/column insertions |
| Opaque — what is B3? | Self-documenting — the name says what it means |
| Not autocompleted in the formula bar | Autocompleted in the formula bar |

**Rule of thumb:** every cell whose value will be referenced anywhere outside
the spreadsheet should have an alias. Intermediate calculation cells that are
only used within the spreadsheet itself do not need one.

---

## Setting an alias

### Via the menu

1. Click the cell (e.g. `B1`).
2. **Spreadsheet → Set Alias**.
3. Type the alias name (e.g. `WallThickness`) and click **OK**.
4. The cell border turns green; the address box shows `B1 (WallThickness)`.

### Via cell properties

1. Right-click the cell → **Properties** → **Cell** tab.
2. Fill in the **Alias** field and click **OK**.

### Via Python

```python
sheet.setAlias("B1", "WallThickness")   # set
sheet.setAlias("B1", "")               # clear
print(sheet.getAlias("B1"))             # read back
```

---

## Alias naming rules

| Rule | Detail |
|------|--------|
| Allowed characters | Letters (A–Z, a–z), digits (0–9), underscore `_` |
| First character | Must be a letter or underscore — not a digit |
| Case-sensitive | `length` and `Length` are distinct aliases |
| No spaces | Use `WallThickness`, not `Wall Thickness` |
| Not a cell address | `A1`, `B3`, etc. are reserved — use descriptive names |
| Not a built-in function | Cannot shadow `sin`, `sum`, `abs`, etc. |

**Recommended style:** PascalCase (`WallThickness`, `BoltHoleDia`) is the most
common convention in FreeCAD community examples. Any consistent style works.

---

## The master-model pattern

The master-model pattern organises a FreeCAD document so that all top-level
design parameters live in **one spreadsheet**, and every geometric feature
references those parameters by alias. The structure looks like this:

```
Document
├── Params (Spreadsheet)         ← all design intent here
│     A             B
│     Length        200 mm       alias: Length
│     Width          80 mm       alias: Width
│     Height         30 mm       alias: Height
│     WallThickness   3 mm       alias: WallThickness
│     BoltHoleDia     6 mm       alias: BoltHoleDia
│     BoltCircleDia  60 mm       alias: BoltCircleDia
│
├── Body
│     ├── BaseSketch (Sketch)
│     │     Horizontal dist.  = Params.Length        ← driven by spreadsheet
│     │     Vertical dist.    = Params.Width
│     ├── Pad                  Length = Params.Height
│     ├── HoleSketch
│     │     Radius             = Params.BoltHoleDia / 2
│     │     Circle dia.        = Params.BoltCircleDia
│     └── Pocket               Type = Through All
│
└── Drawing (TechDraw page)
```

To change any design parameter: **open the spreadsheet, change the value,
recompute**. Every feature that references that alias updates automatically.

---

## Wiring aliases to model dimensions

Any parameter field in FreeCAD that shows a blue `f(x)` expression icon
can accept a spreadsheet alias reference.

### Sketch constraint

1. In sketch edit mode, double-click the constraint to open its dialog.
2. Click the **blue expression icon** (small `f(x)` button) next to the value.
3. Type `Spreadsheet.WallThickness` (or `Params.BoltHoleDia`, etc.) and press Enter.
4. A small blue `f` marker appears on the constraint — it is now driven by the spreadsheet.

### Pad / Pocket depth

1. Open the Pad or Pocket feature (double-click in the tree).
2. Click the expression icon next to **Length**.
3. Type `Spreadsheet.Height` and confirm.

### Any Data-panel property

1. Select the feature in the model tree.
2. Open **View → Panels → Data**.
3. Click the property value once — an expression icon appears.
4. Enter `Spreadsheet.SomeAlias`.

---

## Reading model properties back into the spreadsheet

The link can also run the other direction: a spreadsheet cell can read a
model property as a live computed value.

```
= Pad.Length        ← displays the current Pad depth
= Box.Width         ← displays the current box width
```

Use this for derived quantities — wall areas, volumes, masses — that you
want to display alongside the design parameters.

!!! warning "Never create a cycle"
    If cell A drives parameter P (`P = Spreadsheet.A`), do NOT also write a
    cell formula that reads P back to anything A depends on. FreeCAD detects
    circular dependencies and reports an error on recompute.

---

## Expression syntax reference

Once aliases are set, expressions follow this pattern:

| Situation | Expression |
|-----------|-----------|
| Reference from same spreadsheet | `= WallThickness` (alias alone) |
| Reference from another object | `= Spreadsheet.WallThickness` (object.alias) |
| Reference from a named sheet | `= Params.BoltHoleDia` (if sheet is named "Params") |
| Arithmetic | `= Spreadsheet.Diameter / 2` |
| With units | `= Spreadsheet.Length + 5 mm` |
| Conditional | `= Spreadsheet.UseMetric ? 1.0 : 25.4` |

The `Spreadsheet` part is the object name in the model tree. If you rename
the spreadsheet to `Params`, all references must use `Params.Alias`.

!!! warning "Renaming the spreadsheet updates references automatically"
    FreeCAD re-maps `Spreadsheet.Alias → NewName.Alias` on rename. After
    renaming, recompute the document and verify that no red "Expression error"
    markers appear in the model tree.

---

## Recommended spreadsheet layout

A clean master spreadsheet separates *input parameters* from *derived quantities*:

```
Row  A (label)            B (value)      C (notes / formula preview)
──────────────────────────────────────────────────────────────────
  1  === INPUT PARAMETERS ===
  2  Length                200 mm         alias: Length
  3  Width                  80 mm         alias: Width
  4  Height                 30 mm         alias: Height
  5  WallThickness           3 mm         alias: WallThickness
  6  BoltHoleDia             6 mm         alias: BoltHoleDia
  7
  8  === DERIVED VALUES ===
  9  Inner length           =Length - 2*WallThickness
 10  Inner width            =Width  - 2*WallThickness
 11  Shell volume           =(Length*Width - InnerLength*InnerWidth)*Height
 12  Bolt circle clearance  =BoltCircleDia/2 - BoltHoleDia/2
```

Keep the alias-bearing cells in a predictable section so new team members
know where to look. Use row 1 / column headers as section dividers (plain
text labels without aliases).

---

## Using multiple spreadsheets

A document can contain more than one spreadsheet. Common reasons:

| Sheet name | Contents |
|------------|---------|
| `Params` | Main design parameters |
| `Materials` | Material densities, allowable stresses, Young's moduli |
| `BOM` | Bill of materials — quantities, part numbers, descriptions |
| `Results` | Analysis outputs — volumes, weights, centre of mass |

Reference across sheets with the full object name: `Materials.SteelDensity`,
`Params.WallThickness`.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
sheet = doc.addObject("Spreadsheet::Sheet", "Params")

# Build the parameter table
params = [
    ("A1", "Length",        "B1", "200 mm", "Length"),
    ("A2", "Width",         "B2",  "80 mm", "Width"),
    ("A3", "Height",        "B3",  "30 mm", "Height"),
    ("A4", "WallThickness", "B4",   "3 mm", "WallThickness"),
]
for label_cell, label_text, val_cell, val_text, alias in params:
    sheet.set(label_cell, label_text)
    sheet.set(val_cell, val_text)
    sheet.setAlias(val_cell, alias)

doc.recompute()

# Create a Part Box driven by the spreadsheet
box = doc.addObject("Part::Box", "Box")
box.setExpression("Length", "Params.Length")
box.setExpression("Width",  "Params.Width")
box.setExpression("Height", "Params.Height")

doc.recompute()
print(f"Box: {box.Length} × {box.Width} × {box.Height}")
# → Box: 200.0 mm × 80.0 mm × 30.0 mm

# Change a parameter and recompute
sheet.set("B1", "300 mm")
doc.recompute()
print(f"Box length after change: {box.Length}")
# → Box length after change: 300.0 mm

# List all aliases in the sheet
for addr in ["B1", "B2", "B3", "B4"]:
    alias = sheet.getAlias(addr)
    if alias:
        print(f"  {addr} → {alias}")
```

---

## Common mistakes and pitfalls

!!! warning "Alias on the label cell, not the value cell"
    A typical layout has the label in column A (e.g. `"Wall Thickness"`) and
    the numeric value in column B. Set the alias on the **value cell** (B),
    not the label cell (A). Expressions referencing a text cell that expect a
    number produce a type error.

!!! warning "Model does not update automatically on cell change"
    After editing cell values, press **Ctrl+Shift+R** (or **Model → Refresh**)
    to trigger a full recompute. FreeCAD does not always auto-recompute when a
    cell is changed.

!!! warning "Alias name shadows a cell address"
    An alias of `A1`, `B3`, or any valid cell address is rejected. Choose
    descriptive names that cannot be confused for grid addresses.

!!! warning "Two spreadsheets with the same alias"
    If `Params.Length` and `Materials.Length` both exist, an expression that
    writes `= Length` (without the sheet prefix) may resolve ambiguously.
    Always use the full `SheetName.Alias` form in expressions that cross objects.

---

## See also

- [Set Alias](../tools/aliases.md) — step-by-step alias dialog reference
- [Formulas and Expressions](../tools/formulas-and-expressions.md) — full expression syntax
- [Model Integration](../tools/model-integration.md) — wiring spreadsheet values to geometry
- [Bind Sheet](../tools/bind-sheet.md) — linking spreadsheet cells to model properties bi-directionally
- [Spreadsheet Workbench](../index.md) — workbench overview
