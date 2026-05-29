# Chamfer

> **In one sentence:** Bevel selected edges of a Part shape with a flat angled
> cut, replacing the sharp edge with an angled face.

---

## Overview

**Workbench:** Part · **Menu:** Part → Chamfer  
**Toolbar:** Part Modeling · **Shortcut:** none

Chamfer creates a `Part::Chamfer` object that replaces selected sharp edges with
flat angled faces. It is the "bevel" operation — the flat-cut equivalent of the
rounded [Fillet](fillet.md).

---

## Intuition

Where a Fillet rounds an edge with a circular arc, a Chamfer cuts it with a
flat diagonal face. Chamfers appear in:

- **Machining** — an entry chamfer on a threaded bore guides a bolt into the
  thread.
- **Electronics** — PCB corner chamfers prevent solder bridges.
- **Aesthetics** — chamfered edges give a product a faceted, angular look.

The most common chamfer is equal-distance: both faces are cut back the same
distance (producing a 45° chamfer). Unequal distances produce other angles.

---

## When to use it

- Entry chamfers on bores and counterbores.
- Decorative edge bevels on mechanical or consumer products.
- Replacing filleted edges where a flat face is specifically required.

## When NOT to use it

- **Rounded edge** — use [Fillet](fillet.md).
- **Inside a Part Design Body** — use Part Design Chamfer.

---

## Step-by-step walkthrough

1. **Select the shape** in the model tree.
2. **Part → Chamfer.**
3. In the Chamfer dialog, the shape's edges are listed.
4. **Select the edges** to chamfer.
5. Set the **Size** (chamfer distance from the edge).
6. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Shape | Part object | — | The solid to chamfer |
| Edges | Edge list | — | Edges to chamfer (selected in dialog) |
| Size | Distance | 1 mm | Distance of the chamfer cut from the edge (equal both sides) |
| Size2 | Distance | — | Second distance for unequal chamfer |

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 20; box.Height = 15
doc.recompute()

# Parametric chamfer on all 12 edges
chamfer = doc.addObject("Part::Chamfer", "ChamferedBox")
chamfer.Base = box
# Tuple format: (edge_index, start_size, end_size)
chamfer.Edges = [(i, 1.5, 1.5) for i in range(1, 13)]
doc.recompute()

print(f"Chamfered volume: {chamfer.Shape.Volume:.2f} mm³")
```

---

## Common mistakes and pitfalls

!!! warning "Same failure modes as Fillet"
    Co-planar seam edges from Boolean operations will cause chamfer failures.
    Run [Refine Shape](refine-shape.md) first.

!!! warning "Chamfer size must be smaller than adjacent face"
    If the chamfer size exceeds the width of an adjacent face, OCCT fails.
    Reduce the size.

---

## See also

- [Fillet](fillet.md) — rounded edge (arc cross-section) instead of flat bevel
- [Refine Shape](refine-shape.md) — remove seam edges before chamfering
