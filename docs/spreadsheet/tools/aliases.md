# Aliases (Set Alias)

> **In one sentence:** Assign a human-readable name to a cell so it can be
> referenced as `Spreadsheet.WallThickness` instead of `Spreadsheet.C4` —
> the single most important step for making a parametric spreadsheet
> maintainable.

---

## Overview

**Workbench:** Spreadsheet · **Menu:** Spreadsheet → Set Alias  
**Toolbar:** Spreadsheet · **Shortcut:** none

Set Alias opens a small dialog that lets you type a name for the selected cell.
Once set, the alias appears alongside the cell address everywhere in FreeCAD's
expression engine.

---

## Intuition

Without aliases, a cell reference looks like `Spreadsheet.C4`. This is fragile
and opaque:

- **Fragile:** if you insert a row above row 4, the cell moves to C5 and every
  expression that referenced C4 breaks.
- **Opaque:** reading `Spreadsheet.C4` in a Pad dialog tells you nothing about
  what that cell represents.

With an alias `WallThickness` on C4, the reference becomes
`Spreadsheet.WallThickness`. It:

- **Survives row/column insertion** — the alias travels with the cell, not with
  the address.
- **Self-documents** — every engineer reading the model immediately understands
  what the dimension represents.
- **Is autocompleted** — FreeCAD's formula bar suggests aliases by name.

**Rule of thumb:** Every cell that will be referenced by the model should have
an alias. Cells that are pure intermediate calculations (never referenced
outside the spreadsheet) can stay address-only.

---

## When to use it

- Any cell whose value drives a sketch constraint, Pad/Pocket depth, pattern
  count, or any other model parameter.
- Any cell you plan to reference from another spreadsheet.
- Any cell that represents a named design parameter (rather than an
  intermediate calculation step).

## When NOT to use it

- Intermediate calculation cells that are only referenced by other cells within
  the same spreadsheet — aliases here add noise without value.
- Label cells (A1 = "Length") — the label is plain text; only the value cell
  (B1) needs an alias.

---

## Setting an alias

### Via the menu

1. **Click the cell** you want to alias (e.g. `B1`).
2. **Spreadsheet → Set Alias.**
3. In the dialog, type the alias name (e.g. `WallThickness`).
4. Click **OK**.

The cell address box now shows both the address and the alias: `B1 (WallThickness)`.
The cell border turns green to indicate it has an alias.

### Via the Properties dialog

1. Right-click the cell → **Properties**.
2. Switch to the **Cell** tab.
3. Fill in the **Alias** field.
4. Click **OK**.

---

## Alias naming rules

| Rule | Detail |
|------|--------|
| Allowed characters | Letters (A–Z, a–z), digits (0–9), underscore (`_`) |
| Must start with | A letter or underscore — not a digit |
| Case-sensitive | `length` and `Length` are different aliases |
| No spaces | Use `WallThickness` not `Wall Thickness` |
| No reserved words | Cannot shadow cell addresses (`A1`, `B3`, etc.) or function names (`sin`, `sum`) |

### Recommended naming conventions

- **PascalCase** — `WallThickness`, `BoltHoleDia`, `FlangeLenth`
- **Lower_snake** — `wall_thickness`, `bolt_hole_dia`

Pick one style and use it consistently. PascalCase is most common in FreeCAD
community examples.

---

## Using an alias in expressions

Once set, reference the alias as `<SpreadsheetName>.<Alias>`:

```
Spreadsheet.WallThickness
Params.BoltHoleDia
Materials.SteelDensity
```

In a sketch constraint or Part Design feature dialog, type the full reference
in the expression field:

```
= Spreadsheet.WallThickness
= Spreadsheet.Length / 2
= Spreadsheet.BoltHoleDia + 0.1 mm   # add clearance
```

---

## Clearing an alias

1. Select the cell.
2. **Spreadsheet → Set Alias** → delete the name → **OK**.

Or via Properties → Cell tab → clear the Alias field.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
sheet = doc.addObject("Spreadsheet::Sheet", "Spreadsheet")

# Set values
sheet.set("A1", "Wall Thickness")   # label (plain text, no alias)
sheet.set("B1", "3 mm")             # value

# Set alias on B1
sheet.setAlias("B1", "WallThickness")

doc.recompute()

# Verify
print(sheet.getAlias("B1"))         # "WallThickness"

# Access via alias in another expression
sheet.set("B2", "=WallThickness * 2")   # within same sheet, alias works directly
doc.recompute()
print(sheet.get("B2"))              # 6.0

# Clear alias
sheet.setAlias("B1", "")           # empty string removes the alias
doc.recompute()
```

### Listing all aliases

```python
# Iterate over all cells that have aliases
for cell_addr, cell_obj in sheet.cells.items():
    alias = sheet.getAlias(cell_addr)
    if alias:
        print(f"{cell_addr} → {alias} = {sheet.get(cell_addr)}")
```

---

## Common mistakes and pitfalls

!!! warning "Alias on the label cell, not the value cell"
    A typical spreadsheet layout has a label in column A and the value in
    column B. Set the alias on the **value cell** (B), not the label cell (A).
    Setting an alias on a text cell and then using it in an expression that
    expects a number causes a type error.

!!! warning "Renaming a spreadsheet breaks references"
    If you rename the spreadsheet object (e.g. from `Spreadsheet` to `Params`),
    all external references like `Spreadsheet.WallThickness` become
    `Params.WallThickness`. FreeCAD updates these automatically on rename, but
    check that all external references resolved correctly after renaming.

!!! warning "Alias name conflicts with cell address"
    An alias like `A1` or `B3` is illegal because it conflicts with the cell
    address syntax. FreeCAD will refuse to set it. Use descriptive names that
    cannot be mistaken for addresses.

---

## See also

- [Formulas and Expressions](formulas-and-expressions.md) — syntax for using aliases in formulas
- [Model Integration](model-integration.md) — feeding aliased values into model dimensions
- [Create Spreadsheet](create-spreadsheet.md) — setting up the spreadsheet before adding aliases
