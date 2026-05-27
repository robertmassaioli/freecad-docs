# Cube

> **In one sentence:** Create a parametric rectangular box (cuboid) whose
> length, width, and height can be changed at any time.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Cube  
**Toolbar:** Part Primitives · **Shortcut:** none

The Cube primitive creates a rectangular box aligned with the world XYZ axes.
All three dimensions remain live parameters in the Data panel — change them and
the shape updates immediately without rebuilding.

---

## Intuition

Every new FreeCAD model needs a starting shape. Cube is the fastest way to
get a rectangular solid into the scene. From that first box, you subtract
cylinders (holes), fuse flanges, or import STEP geometry and Boolean-combine
everything into a finished part.

Unlike Part Design's Additive Box, a Part Cube does not need to live inside a
Body container. It is a standalone shape that can be combined with any other
Part object via Cut / Fuse / Common.

---

## When to use it

- You need a quick rectangular block to cut holes into or union with other shapes.
- You are scripting geometry and want the simplest parametric solid.
- You are working outside a Part Design Body (imported STEP workflow).

## When NOT to use it

- **You are inside a Part Design Body** — use Additive Box instead; it chains
  correctly in the feature tree.
- **You need a hollow tube** — use [Tube](tube.md) or subtract a smaller
  cylinder from a larger one.

---

## Step-by-step walkthrough

1. **Switch to the Part workbench.**
2. **Part → Primitives → Cube** (or click the Cube toolbar button).
3. A 10 × 10 × 10 mm box appears at the world origin.
4. Select it in the model tree and open the **Data** panel.
5. Edit **Length**, **Width**, **Height** to your required dimensions.
6. Optionally set **Placement** to position the box.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Length | Distance | 10 mm | Extent along the X axis |
| Width | Distance | 10 mm | Extent along the Y axis |
| Height | Distance | 10 mm | Extent along the Z axis |
| Placement | Placement | Identity | Position and orientation of the box |

> **Note:** The box always starts at its corner (0, 0, 0) relative to its own
> placement. Its opposite corner is at (Length, Width, Height). Use Placement
> to centre or rotate it.

---

## Common mistakes and pitfalls

!!! warning "Box at origin corner, not centred"
    By default the box occupies (0,0,0)→(L,W,H). If you expect it centred,
    set the Placement position to (−L/2, −W/2, −H/2) or use Part Mirror /
    Part Move to reposition.

!!! warning "Confusing Part Cube with Part Design Additive Box"
    Part Cube is a standalone shape. Part Design Additive Box lives inside a
    Body and participates in the feature chain. They look identical but behave
    differently in the tree.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Create a parametric cube
box = doc.addObject("Part::Box", "MyCube")
box.Length = 20.0   # mm
box.Width  = 15.0   # mm
box.Height = 10.0   # mm

# Reposition the box so it is centred on the origin
box.Placement = App.Placement(
    App.Vector(-10, -7.5, -5),   # position
    App.Rotation()               # no rotation
)

doc.recompute()
print(box.Shape.Volume)   # 3000.0
```

### Shape access

```python
# Access the underlying OCCT shape
shape = box.Shape

# Faces (6 faces on a box)
for i, face in enumerate(shape.Faces):
    print(f"Face {i}: area = {face.Area:.2f} mm²")

# Bounding box
bb = shape.BoundBox
print(f"Dimensions: {bb.XLength} × {bb.YLength} × {bb.ZLength} mm")
```

---

## See also

- [Cylinder](cylinder.md) — circular cross-section primitive
- [Boolean Cut](boolean-cut.md) — subtract cylinders to make holes
- [Boolean Fuse](boolean-fuse.md) — merge two boxes into an L-shape
- [Shape Builder](shape-builder.md) — compose shapes from faces
