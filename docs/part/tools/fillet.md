# Fillet

> **In one sentence:** Round selected edges of a Part shape with a constant or
> variable radius to remove sharp corners.

---

## Overview

**Workbench:** Part · **Menu:** Part → Fillet  
**Toolbar:** Part Modeling · **Shortcut:** none

Fillet creates a `Part::Fillet` object that applies curved, round transitions
along the selected edges of a shape. Unlike Part Design Fillet — which operates
on the Body tip — Part Fillet operates on any standalone Part shape.

---

## Intuition

Sharp edges cause stress concentrations, are hard to deburr in manufacturing,
and are hazardous to handle. Fillets replace the sharp edge with a smooth
circular arc cross-section. In addition to structural benefits, fillets make
parts look more polished and injection-moulded.

---

## When to use it

- Rounding external corners of a machined block.
- Applying draft-radius fillets before injection mould analysis.
- Softening internal corners of a cut feature.

## When NOT to use it

- **Inside a Part Design Body** — use Part Design Fillet on the Body tip.
- **Bevelled edge (flat, not rounded)** — use [Chamfer](chamfer.md).
- **Edges that are tangent or near-tangent** — OCCT may fail to fillet them.

---

## Step-by-step walkthrough

1. **Select the shape** (the whole solid, not individual edges) in the model tree.
2. **Part → Fillet.**
3. In the Fillet dialog, the shape's edges are listed.
4. **Select the edges** to fillet (Ctrl+click for multiple).
5. Set the **Radius**.
6. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Shape | Part object | — | The solid to fillet |
| Edges | Edge list | — | Edges to fillet (selected in dialog) |
| Radius | Distance | 1 mm | Fillet radius |

---

## Common mistakes and pitfalls

!!! warning "Fillet failure on co-planar edges"
    After a Boolean Fuse, the merged solid may have internal seam edges where
    two faces were originally co-planar. Fillets on these edges fail.
    Use [Refine Shape](copy-tools.md#refine-shape) before filleting.

!!! warning "Radius too large — fillet exceeds the face size"
    If the fillet radius is larger than the adjacent face allows, OCCT raises
    an error. Reduce the radius or shorten the adjacent face.

!!! warning "Select the shape, not individual edges, when launching the tool"
    Fillet operates on the parent shape and lets you select edges inside the
    dialog. If you pre-select edges before launching, the dialog may not show
    all available edges.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 20; box.Height = 15
doc.recompute()

# Parametric fillet via Part::Fillet object
fillet = doc.addObject("Part::Fillet", "RoundedBox")
fillet.Base = box
# Select all 12 edges with radius 2 mm
fillet.Edges = [(i, 2.0, 2.0) for i in range(1, 13)]  # (edge_index, start_r, end_r)
doc.recompute()

print(f"Filleted volume: {fillet.Shape.Volume:.2f} mm³")
```

> **Note:** Edge indices in the `Edges` property are 1-based. The tuple format
> is `(edge_index, start_radius, end_radius)` — use the same value for both
> for a constant-radius fillet.

---

## See also

- [Chamfer](chamfer.md) — bevel (flat cut) instead of round
- [Refine Shape](copy-tools.md#refine-shape) — remove co-planar seams before filleting
- [Check Geometry](check-geometry.md) — verify the fillet result
