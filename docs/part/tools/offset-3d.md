# Offset 3D Surface

> **In one sentence:** Offset all faces of a solid outward (positive) or inward
> (negative) by a uniform distance, enlarging or shrinking the entire shape.

---

## Overview

**Workbench:** Part · **Menu:** Part → Offset 3D Surface  
**Toolbar:** Part Modeling · **Shortcut:** none

Offset 3D Surface moves every face of a shape outward or inward by a constant
distance. It is the 3-D equivalent of inflating or deflating a balloon by a
fixed amount. The result is a shape of the same topology but different
dimensions.

---

## Intuition

If you design a part and then discover the wall must be 1 mm thicker on the
outside, offsetting the outer surface by 1 mm avoids having to rebuild the
geometry. Offset 3D is especially useful with imported STEP geometry where
no parametric features exist to change directly.

Positive offset (outward) **enlarges** the shape. Negative offset (inward)
**shrinks** it. A negative offset equal to the wall thickness hollows out the
solid.

---

## When to use it

- Adding or removing uniform material from an imported solid.
- Generating an offset envelope for clearance or interference calculations.
- Creating a shell by offsetting inward (see also [Thickness](thickness.md)).

## When NOT to use it

- **Offsetting only one face** — use a dedicated face-level offset workflow.
- **Offsetting a 2-D wire or face in its plane** — use [Offset 2D](offset-2d.md).

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Offset | Distance | 1 mm | Offset distance (positive = outward, negative = inward) |
| Tolerance | Distance | 1e-7 | OCCT internal tolerance for offset computation |
| Intersection | Bool | No | How self-intersections are handled |
| Self intersection | Bool | No | Allow self-intersecting result |
| Fill Offset | Bool | No | Fill the gap between original and offset surfaces |
| Mode | Skin / Pipe / RectoVerso | Skin | Surface construction mode |

---

## Step-by-step walkthrough

1. **Select the solid** to offset.
2. **Part → Offset 3D Surface.**
3. In the dialog, set the **Offset** distance.
4. Choose **Mode** (Skin is almost always the right choice).
5. Click **OK**.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 20; box.Height = 15
doc.recompute()

# Offset outward by 2 mm
offset = doc.addObject("Part::Offset3D", "Enlarged")
offset.Source = box
offset.Value  = 2.0       # mm, positive = outward
offset.Mode   = "Skin"    # skin / pipe
offset.Intersection = False
doc.recompute()

print(f"Original volume: {box.Shape.Volume:.2f} mm³")
print(f"Enlarged volume: {offset.Shape.Volume:.2f} mm³")
```

---

## Common mistakes and pitfalls

!!! warning "Negative offset larger than smallest feature"
    An inward offset larger than the smallest radius of curvature or wall
    thickness causes OCCT topology errors. Reduce the offset or simplify the
    shape first.

!!! warning "Offset 3D vs Thickness"
    Offset 3D offsets all faces uniformly and keeps the solid closed.
    [Thickness](thickness.md) removes one or more faces and creates a hollow
    shell. For hollowing, use Thickness.

---

## See also

- [Offset 2D](offset-2d.md) — offset a 2-D wire or face in its plane
- [Thickness](thickness.md) — hollow out a solid with open faces
- [Scale](scale.md) — non-uniform scaling (different from offsetting)
