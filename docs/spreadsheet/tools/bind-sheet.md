# Bind Sheet

> **In one sentence:** Synchronise a rectangular range of cells in the current
> spreadsheet to a matching range in another spreadsheet — keeping the two
> ranges in lock-step without manual copying.

---

## Overview

**Workbench:** Spreadsheet · **Menu:** Spreadsheet → Bind Sheet (right-click
a selection → Bind…)  
**Toolbar:** none · **Shortcut:** none

The Bind Sheet dialog links a selected range of cells to a source range in
another (or the same) spreadsheet. The bound cells mirror the source values
automatically on recompute.

---

## Intuition

Imagine a project with several sub-assemblies, each in its own FreeCAD document,
and a master document that aggregates key dimensions from all of them. Without
Bind Sheet, you would manually copy values between spreadsheets — a maintenance
nightmare. With Bind Sheet, the master spreadsheet binds directly to the source
ranges in each sub-assembly's spreadsheet, and the values update automatically
when the sub-assemblies change.

Within a single document, Bind Sheet is useful for:

- **Lookup tables** — bind a range that references a material library to a
  local working range.
- **Derived parameter sheets** — one spreadsheet holds master parameters;
  a second binds to a subset for a specific sub-system.
- **Read-only display copies** — bind to a formula sheet so one view is editable,
  the other is display-only.

---

## When to use it

- Synchronising parameter ranges between two spreadsheets in the same document.
- Linking to ranges in an external document (another `.FCStd` file).
- Creating a read-only mirror of an editable parameter range.

## When NOT to use it

- **You only need one or two values** — a regular cell formula
  `=OtherSheet.AliasName` is simpler and more explicit.
- **You need computed/transformed values** — Bind Sheet mirrors values as-is.
  For transformed data (scaled, offset), use formulas.

---

## Step-by-step walkthrough

1. **Select the destination range** — the cells in the *current* spreadsheet
   where the bound values should appear.
2. **Right-click the selection → Bind…** (or Spreadsheet → Bind Sheet).
3. In the Bind Sheet dialog:
   - **Target spreadsheet** — the spreadsheet object to bind *from*.
   - **Start cell / End cell** — the source range in the target spreadsheet.
   - **Binding type:**
     - *Normal* — the bound cells mirror the source values; they cannot be
       edited directly.
     - *Hidden reference* — like Normal but the dependency is not shown in the
       dependency graph (for use with circular-reference workarounds).
4. Click **OK**.
5. The bound cells show the values from the source range and are marked
   read-only (slightly different appearance).

---

## Binding types

### Normal binding

The destination cells are read-only mirrors of the source. Any expression in
the source is evaluated; the destination shows the result.

FreeCAD tracks the dependency: if the source changes, the destination updates
on recompute.

### Hidden reference binding

Like Normal but the dependency link is hidden from FreeCAD's dependency tracker.
This is an advanced option used when you need to read values from a spreadsheet
that would otherwise create a circular dependency in the regular dependency
graph. Use with care — hidden references can mask true circular dependencies.

---

## Clearing a binding

1. Select the bound range.
2. **Right-click → Bind…** → click **Discard** (or **Remove binding** in the
   dialog).
3. The cells become regular editable cells again (content is the last mirrored value).

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Source spreadsheet
src = doc.addObject("Spreadsheet::Sheet", "Source")
src.set("A1", "100 mm"); src.setAlias("A1", "BaseLength")
src.set("A2", "50 mm");  src.setAlias("A2", "BaseWidth")
src.set("A3", "20 mm");  src.setAlias("A3", "BaseHeight")
doc.recompute()

# Destination spreadsheet
dst = doc.addObject("Spreadsheet::Sheet", "Mirror")
doc.recompute()

# Bind A1:A3 in dst to A1:A3 in src
# Expression-based equivalent (simpler for small ranges):
dst.set("A1", "=Source.BaseLength")
dst.set("A2", "=Source.BaseWidth")
dst.set("A3", "=Source.BaseHeight")
doc.recompute()

print(dst.get("A1"))   # 100.0 (mirrors Source.BaseLength)

# Change source value
src.set("A1", "150 mm")
doc.recompute()
print(dst.get("A1"))   # 150.0 (auto-updated)
```

> **Note:** The `Bind Sheet` dialog creates a range binding using FreeCAD's
> internal tuple-expression syntax. The Python equivalent above uses
> individual cell expressions, which are equivalent for small ranges. For
> large range bindings, use the GUI.

---

## Common mistakes and pitfalls

!!! warning "Bound cells cannot be edited directly"
    Once a range is bound, the destination cells are read-only. Attempts to
    edit them directly are silently ignored. To edit: clear the binding first.

!!! warning "Source and destination range sizes must match"
    The destination selection and the source range must have the same number
    of rows and columns. A mismatch causes an error in the Bind dialog.

!!! warning "Hidden reference can mask circular dependencies"
    Using Hidden Reference to work around a circular dependency error usually
    means the model design has a real logical cycle. Resolve the dependency
    structure instead of hiding it.

---

## See also

- [Formulas and Expressions](formulas-and-expressions.md) — simpler alternative for individual cell references
- [Aliases](aliases.md) — access bound values by name
- [Model Integration](model-integration.md) — driving model parameters from bound spreadsheets
