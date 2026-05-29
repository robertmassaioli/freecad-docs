# Transformed Copy

> **In one sentence:** Create a static copy of a shape with its current
> Placement baked into the geometry, so the copy's Placement is identity.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Create a Copy → Transformed Copy  
**Shortcut:** none

Creates a static copy of the selected shape with the shape's current Placement
matrix applied directly to the vertex coordinates. The resulting shape has an
identity Placement — its coordinates are in world space.

---

## Intuition

FreeCAD separates *shape geometry* (vertex coordinates relative to the shape's
own origin) from *Placement* (the matrix that positions the shape in world
space). Normally, changing the Placement just moves the shape without altering
its internal coordinates.

Transformed Copy "flattens" this separation: it applies the current Placement
to all vertex coordinates, then resets the Placement to identity. The result
is a shape that geometrically occupies the same world-space position but no
longer depends on a Placement matrix.

Use it when you want to export a rotated or translated shape and have the
output file reflect the absolute world-space coordinates.

---

## When to use it

- You placed a shape at a non-zero position or rotation and want to export it
  with those coordinates baked into the geometry.
- You want a "flattened" shape with no placement offset for a downstream
  Boolean operation.
- You need to hand off a shape whose vertices are already in world space (e.g.,
  for CNC toolpath generation from absolute coordinates).

## When NOT to use it

- **Don't use it when you need the copy to remain parametric** — use
  [Simple Copy](simple-copy.md) or a parametric clone instead.
- **Don't use it just to reset the Placement** — you can zero the Placement
  property directly without creating a copy.

---

## Step-by-step

1. Set the shape's Placement to the desired world-space position/orientation.
2. Select the shape in the tree or 3D view.
3. Choose **Part → Create a Copy → Transformed Copy**.
4. A new `Part::Feature` appears with Placement = identity and geometry at the
   correct world coordinates.

---

## Parameters

This tool has no task panel. The copy is created immediately on activation.

---

## Common mistakes and pitfalls

!!! warning "The copy does not update"
    Transformed Copy is static. If you later change the original's Placement
    or geometry, the copy is unaffected.

!!! warning "Zero-offset shapes are unchanged"
    If the original already has an identity Placement, Transformed Copy
    produces a copy identical to Simple Copy.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Placement = App.Placement(
    App.Vector(10, 20, 5),
    App.Rotation(App.Vector(0, 0, 1), 45)  # 45° rotation around Z
)
doc.recompute()

# Bake the placement into the shape geometry
transformed = box.Shape.copy()
transformed.Placement = App.Placement()   # reset to identity

feat = doc.addObject("Part::Feature", "TransformedCopy")
feat.Shape = transformed
doc.recompute()

# The copy occupies the same world position, but Placement is now identity
print(feat.Placement)   # (0, 0, 0), no rotation
```

---

## See also

- [Simple Copy](simple-copy.md) — plain non-parametric copy (does not bake placement)
- [Shape Element Copy](shape-element-copy.md) — copy a single sub-element
- [Refine Shape](refine-shape.md) — clean up topology after copying
- [Part Workbench](../index.md) — workbench overview
