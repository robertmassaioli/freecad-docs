# Refine Shape

> **In one sentence:** Create a cleaned-up copy of a shape by merging
> co-planar faces and removing redundant internal edges left by Boolean
> operations.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Create a Copy → Refine Shape  
**Shortcut:** none

Creates a new `Part::Feature` with the same overall geometry as the selected
shape, but with redundant internal edges removed and adjacent co-planar faces
merged into single faces.

---

## Intuition

When you perform a Boolean Fuse or Cut in FreeCAD, OCCT preserves all the
original face boundaries in the result — including seam edges where two
faces are now perfectly co-planar (they could be one face, but the history says
they were two). Refine Shape merges them, producing cleaner geometry with fewer
faces.

Think of it as erasing pencil guide lines after the drawing is done — the
shape looks the same, but the unnecessary lines are gone.

This is particularly important before applying Fillet or Chamfer: excess edges
on planar regions can cause fillet failures because OCCT tries to apply a fillet
to a zero-curvature boundary.

---

## When to use it

- After Boolean Fuse, Boolean Cut, or Boolean Common operations, to clean up
  redundant edges on planar regions.
- Before applying Fillet or Chamfer to a Boolean result.
- Before export (STEP, STL) when you want a minimal face count.
- After [Shape from Mesh](shape-from-mesh.md) to merge co-planar triangles
  into single planar faces.

## When NOT to use it

- **Don't use it on shapes with intentionally coincident edges** — if a face
  must remain as two separate faces for downstream selection or reference,
  refining will merge them.
- **Don't use it if you need to reference specific faces by index** — after
  refining, face indices change because faces are merged. Any existing references
  to the old face indices will become invalid.

---

## Step-by-step

1. Select the shape to refine (typically a Boolean result).
2. Choose **Part → Create a Copy → Refine Shape**.
3. A new `Part::Feature` appears with the cleaned topology.
4. Inspect the result: the number of faces should be less than or equal to the
   original.

---

## Parameters

This tool has no task panel. The refinement is applied automatically to the
selected shape.

---

## Common mistakes and pitfalls

!!! warning "Face count change invalidates downstream references"
    If any other features (sketches, fillets, other Booleans) reference faces
    of the original by index, those references may break after refining because
    face indices change. Create the refined shape before adding downstream
    references.

!!! warning "Refine Shape does not fix invalid topology"
    It cleans up co-planar seam edges, but it cannot repair self-intersections,
    open shells, or non-manifold geometry. Use [Check Geometry](check-geometry.md)
    for diagnosis.

!!! warning "May not merge curved faces"
    Refine Shape merges co-planar faces and tangentially continuous curved faces.
    Disjoint curved faces that happen to share a seam edge may not always merge
    as expected.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a Boolean operation that produces redundant edges
box1 = doc.addObject("Part::Box", "Box1")
box1.Length = 20; box1.Width = 10; box1.Height = 10

box2 = doc.addObject("Part::Box", "Box2")
box2.Length = 20; box2.Width = 10; box2.Height = 10
box2.Placement = App.Placement(App.Vector(20, 0, 0), App.Rotation())

fuse = doc.addObject("Part::Fuse", "Fused")
fuse.Base = box1
fuse.Tool = box2
doc.recompute()

print(f"Faces before refine: {len(fuse.Shape.Faces)}")   # more faces with seam edges

# Refine the shape using removeSplitter()
refined_shape = fuse.Shape.removeSplitter()
feat = doc.addObject("Part::Feature", "Refined")
feat.Shape = refined_shape
doc.recompute()

print(f"Faces after refine: {len(feat.Shape.Faces)}")    # fewer, cleaner faces
```

---

## See also

- [Simple Copy](simple-copy.md) — plain non-parametric copy without refinement
- [Boolean Fuse](boolean-fuse.md) — merge shapes (produces redundant edges; refine afterwards)
- [Boolean Cut](boolean-cut.md) — subtract shapes (same advice)
- [Check Geometry](check-geometry.md) — verify shape validity after refinement
- [Shape from Mesh](shape-from-mesh.md) — combine with Refine to clean up mesh-converted shapes
- [Part Workbench](../index.md) — workbench overview
