# Cone

> **In one sentence:** Create a parametric cone or frustum (truncated cone)
> with configurable radii, height, and angular sweep.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Cone  
**Toolbar:** Part Primitives · **Shortcut:** none

The Cone primitive creates a right circular cone or a conical frustum (a cone
with the tip cut off). Setting Radius1 = 0 gives a pointed cone; setting
both radii to different non-zero values gives a frustum.

---

## Intuition

Cones appear as:
- **Chamfer preview shapes** — a short frustum at the entrance of a hole.
- **Funnel or hopper shapes** — a frustum open at both ends (make hollow
  with Thickness or use a shell).
- **Tapered pins** — a cone as a positive solid.
- **Partial sectors** — with Angle < 360°.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Distance | 2 mm | Radius at the base (Z = 0) |
| Radius2 | Distance | 4 mm | Radius at the top (Z = Height). Set to 0 for a pointed tip. |
| Height | Distance | 10 mm | Axial height along Z |
| Angle | Angle | 360° | Longitude sweep |
| Placement | Placement | Identity | Position and orientation |

> **Note:** The default produces a frustum with Radius1 < Radius2 (wider at
> top). To get a cone narrowing upward, set Radius1 > Radius2 (or Radius2 = 0
> for a true pointed cone).

---

## Step-by-step walkthrough

1. **Part → Primitives → Cone.**
2. A frustum appears (Radius1 = 2, Radius2 = 4, Height = 10 mm).
3. In **Data**, set **Radius1** to the bottom radius and **Radius2** to the
   top radius. Set **Radius2 = 0** for a pointed cone.
4. Adjust **Height** and **Placement**.

---

## Common mistakes and pitfalls

!!! warning "Default Cone is a frustum, not a pointed cone"
    The factory defaults produce a wider-at-top frustum. Set Radius2 = 0 if
    you want a pointed apex.

!!! warning "Cone axis is Z"
    Like Cylinder, the cone axis is along local Z. Rotate Placement to
    reorient.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Pointed cone
cone = doc.addObject("Part::Cone", "PointedCone")
cone.Radius1 = 10.0  # base radius
cone.Radius2 = 0.0   # pointed tip
cone.Height  = 20.0

doc.recompute()
print(f"Volume: {cone.Shape.Volume:.2f} mm³")

# Frustum (truncated cone)
frustum = doc.addObject("Part::Cone", "Frustum")
frustum.Radius1 = 20.0  # wide base
frustum.Radius2 = 10.0  # narrower top
frustum.Height  = 15.0

doc.recompute()
```

---

## See also

- [Cylinder](cylinder.md) — constant-radius cylinder
- [Sphere](sphere.md) — spherical primitive
- [Tube](tube.md) — hollow cylinder
