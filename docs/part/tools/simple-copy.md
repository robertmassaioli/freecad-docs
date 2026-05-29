# Simple Copy

> **In one sentence:** Create a static, non-parametric snapshot of a shape
> that never updates when the original changes.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Create a Copy → Simple Copy  
**Shortcut:** none

Creates a fully detached copy of the selected shape. The copy is frozen at the
moment of creation — it does not update if the original shape is modified.

---

## Intuition

Simple Copy breaks the parametric link between the copy and its source. Think of
it as taking a photograph: the photo records what the object looked like at that
instant, but it will never change if the object changes later.

This is useful when you need a stable reference shape for export, or when you
want to branch a design by making two independent variants from the same starting
point.

---

## When to use it

- You want to lock in the current state of a shape before further modifications.
- You need to hand off a shape to an external tool that cannot handle live Part
  features.
- You want two independent design variants from the same starting geometry.
- You need to export a shape that should not be affected by future model changes.

## When NOT to use it

- **Don't use it if you need the copy to track the original** — use Part Mirror,
  Part Array, or a PartDesign feature with a reference instead.
- **Don't use it as a workaround for over-complex models** — if your model is
  sluggish, optimise the feature tree rather than freezing shapes.

---

## Step-by-step

1. Select the shape you want to copy in the 3D view or model tree.
2. Choose **Part → Create a Copy → Simple Copy**.
3. A new `Part::Feature` object appears in the tree containing a static copy.
4. Modify the original — the copy remains unchanged.

---

## Parameters

This tool has no task panel. The copy is created immediately on activation.

---

## Common mistakes and pitfalls

!!! warning "Copy does not update"
    This is intentional behaviour, not a bug. If you modify the original and
    expect the copy to update, use a different tool (e.g., Draft Clone or a
    parametric mirror/array).

!!! warning "Face indices may differ from original"
    The topology of the copy is the same as the original at the moment of
    copying, but face/edge indices may shift in subsequent model changes
    if you later reference them.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

original = doc.addObject("Part::Box", "Original")
original.Length = 20
original.Width = 15
original.Height = 10
doc.recompute()

# Create a static copy
copy_shape = original.Shape.copy()
copy_feat = doc.addObject("Part::Feature", "StaticCopy")
copy_feat.Shape = copy_shape
doc.recompute()

# Modify the original — the copy is unaffected
original.Length = 40
doc.recompute()
print(f"Copy length: {copy_feat.Shape.BoundBox.XLength}")   # still 20.0
```

---

## See also

- [Transformed Copy](transformed-copy.md) — copy with placement baked into geometry
- [Shape Element Copy](shape-element-copy.md) — copy a single face, edge, or vertex
- [Refine Shape](refine-shape.md) — clean up redundant edges after Boolean operations
- [Part Workbench](../index.md) — workbench overview
