# Boolean Dialog

> **In one sentence:** A general-purpose Boolean dialog that lets you choose
> the operation type (Cut, Fuse, Common, or Section) and both operand shapes
> from a single panel.

---

## Overview

**Workbench:** Part · **Menu:** Part → Boolean…  
**Toolbar:** Part Boolean · **Shortcut:** none

The Boolean dialog is the "manual" Boolean tool. Unlike the dedicated
[Cut](boolean-cut.md), [Fuse](boolean-fuse.md), and
[Common](boolean-common.md) buttons — which operate on the pre-selected shapes
— this dialog lets you pick both shapes and the operation type interactively.

---

## Intuition

Use the Boolean dialog when:

- You have not pre-selected the shapes and want to pick them inside the dialog.
- You want to quickly switch between Cut / Fuse / Common / Section without
  creating multiple result objects.
- You are scripting and want a single object type that records the operation.

For repeated or common operations, the dedicated tools (Cut, Fuse, Common) are
faster because they pick up the selection automatically.

---

## When to use it

- Ad-hoc Boolean between two shapes where you want to see the operation type
  chosen from a dropdown before committing.
- Prototyping — quickly try Cut vs Fuse to see which gives the right result.

## When NOT to use it

- **Repeated identical operations** — use the dedicated [Cut](boolean-cut.md),
  [Fuse](boolean-fuse.md), or [Common](boolean-common.md) buttons.
- **More than two shapes** — use [Boolean Fragments](boolean-fragments.md)
  for multi-shape operations.

---

## Operations

| Operation | Result |
|-----------|--------|
| **Cut** | Subtract Tool shape from Base shape |
| **Fuse** | Union of Base and Tool |
| **Common** | Intersection (volume shared by both) |
| **Section** | Wire where the two surfaces intersect |

---

## Step-by-step walkthrough

1. **Part → Boolean…**
2. In the dialog, select the **Base Shape** from the dropdown (first operand).
3. Select the **Tool Shape** (second operand).
4. Choose the **Operation** type.
5. Click **OK** — the result shape appears.

---

## Parameters

| Field | Description |
|-------|-------------|
| Base Shape | First operand (the shape being modified) |
| Tool Shape | Second operand (the modifier) |
| Operation | Cut / Fuse / Common / Section |

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 20; box.Height = 20

cyl = doc.addObject("Part::Cylinder", "Cyl")
cyl.Radius = 8; cyl.Height = 25
cyl.Placement = App.Placement(App.Vector(15, 10, -2), App.Rotation())
doc.recompute()

# Boolean Cut via object type
cut = doc.addObject("Part::Cut", "Hole")
cut.Base = box
cut.Tool = cyl
doc.recompute()

# Boolean Fuse
fuse = doc.addObject("Part::Fuse", "Union")
fuse.Base = box
fuse.Tool = cyl
# doc.recompute()  # uncomment if using fuse

# Boolean Common
common = doc.addObject("Part::Common", "Intersection")
common.Base = box
common.Tool = cyl
# doc.recompute()  # uncomment if using common
```

---

## See also

- [Boolean Cut](boolean-cut.md) — dedicated cut button
- [Boolean Fuse](boolean-fuse.md) — dedicated fuse button
- [Boolean Common](boolean-common.md) — dedicated common button
- [Boolean Fragments](boolean-fragments.md) — split all
  intersecting shapes simultaneously
