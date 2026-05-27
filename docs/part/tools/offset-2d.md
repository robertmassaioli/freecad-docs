# Offset 2D

> **In one sentence:** Offset a 2-D wire or face inward or outward within its
> plane — the 2-D equivalent of inflating or deflating a closed profile.

---

## Overview

**Workbench:** Part · **Menu:** Part → Offset 2D  
**Toolbar:** Part Modeling · **Shortcut:** none

Offset 2D offsets a 2-D closed wire or face in its own plane by a uniform
distance. The result is a new wire or face at the specified offset. This is the
planar equivalent of [Offset 3D](offset-3d.md).

---

## Intuition

Given a square profile, Offset 2D with a positive value produces a larger
square with rounded or chamfered corners. With a negative value it produces a
smaller square. This is the same operation performed by a router following an
offset toolpath, or by the "expand/contract selection" operation in a vector
graphics editor.

Common uses:
- Generating a clearance profile around a 2-D outline.
- Creating a slightly smaller inner profile for a snap-fit feature.
- Offsetting a sketch profile before extruding (instead of changing the sketch
  dimensions one by one).

---

## When to use it

- You have a 2-D wire or face and need a uniformly grown or shrunk version of it.
- Creating a wall-thickness offset of a 2-D profile before extruding.

## When NOT to use it

- **3-D shape offset** — use [Offset 3D](offset-3d.md).
- **Non-uniform scaling** — use [Scale](scale.md).

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Source | Wire or Face | — | The 2-D profile to offset |
| Value | Distance | 1 mm | Offset distance (positive = outward, negative = inward) |
| Mode | Skin / Pipe | Skin | Outer face handling |
| Join | Arc / Tangent / Intersect | Arc | How convex corners are joined |
| Inner Mode | Ignore / Fill | Ignore | Handling of inner (concave) corners |
| Open Result | Bool | No | Allow open result wire (no closing caps) |

### Join modes

| Mode | Description |
|------|-------------|
| Arc | Convex corners are rounded with an arc of the offset radius |
| Tangent | Convex corners are extended to tangent intersection (sharp miter) |
| Intersect | Convex corners are extended to their intersection (miter, may spike) |

---

## Step-by-step walkthrough

1. **Select the 2-D wire or face** in the viewport.
2. **Part → Offset 2D.**
3. Set the **Value** (positive to grow, negative to shrink).
4. Choose the **Join** mode for how convex corners are treated.
5. Click **OK**.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Square wire in XY plane
p1 = App.Vector(0, 0, 0); p2 = App.Vector(20, 0, 0)
p3 = App.Vector(20, 20, 0); p4 = App.Vector(0, 20, 0)
wire = Part.Wire([
    Part.makeLine(p1, p2), Part.makeLine(p2, p3),
    Part.makeLine(p3, p4), Part.makeLine(p4, p1)
])
face = Part.Face(wire)

# Offset outward by 3 mm with arc corners
offset_shape = face.makeOffset2D(3.0, join=0)  # join 0=Arc, 1=Tangent, 2=Intersect
feat = doc.addObject("Part::Feature", "OffsetFace")
feat.Shape = offset_shape
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Negative offset past the inscribed circle"
    If the inward offset is larger than the inscribed circle radius of the
    profile, the profile collapses. For a square of side 20 mm, the maximum
    inward offset is 10 mm.

---

## See also

- [Offset 3D](offset-3d.md) — offset a 3-D solid
- [Thickness](thickness.md) — hollow out a solid (3-D shell operation)
- [Extrude](extrude.md) — extrude the offset face into a solid
