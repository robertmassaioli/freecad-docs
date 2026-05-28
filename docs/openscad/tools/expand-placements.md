# Expand Placements

> **In one sentence:** Push placement values from parent objects down into
> their children, baking absolute positions into each child and resetting the
> parent to the origin.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Expand Placements  
**Shortcut:** none

Propagates the placement matrix from each object downward through its children.
After this operation each child has an absolute world-space placement and its
parent sits at the origin.

---

## Intuition

Imagine a set of Russian dolls where each outer doll holds the inner doll in
a specific position. Expand Placements takes the "where I hold my child" offset
from each doll and adds it permanently to each child's own "where I am" value —
so each doll now knows its absolute position independently, and all the nesting
offsets are zeroed out.

---

## When to use it

- You are preparing geometry for export and need absolute (world-space)
  coordinates in each object.
- A downstream operation requires objects at the origin.
- You want to simplify a deeply nested placement hierarchy.

---

## When NOT to use it

- **On assemblies with live constraints** — this operation bakes placements
  and can break constraints or expressions that depend on relative placement.
- **When reversibility is needed** — the original relative placements are
  not preserved.

---

## Step-by-step

1. Select the root object(s) of the placement hierarchy to expand.
2. Choose **OpenSCAD → Expand Placements**.
3. Each child object's placement is updated to the world-space position.
4. The parent placement is reset to the origin.

---

## Common mistakes and pitfalls

!!! warning "Extrusions and dependent features break after expansion"
    **Cause:** Features like Pad or Pocket depend on relative placement of
    their sketch. Baking the placement changes the coordinate frame.  
    **Fix:** Use Expand Placements only on final geometry intended for export,
    not on live Part Design features.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_ExpandPlacements', 0)")
```

---

## See also

- [Explode Group](explode-group.md) — break a compound into individual objects
- [OpenSCAD Workbench](../index.md) — workbench overview
