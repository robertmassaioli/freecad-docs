# Cylinder

> **In one sentence:** Create a parametric cylinder with a configurable radius,
> height, and angular sweep that can be changed at any time.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Cylinder  
**Toolbar:** Part Primitives · **Shortcut:** none

The Cylinder primitive creates a right circular cylinder (or a partial cylinder
sector if Angle < 360°). Radius and Height are live parameters.

---

## Intuition

Cylinders are the workhorse of mechanical part modelling: shafts, bosses,
through-holes (as cutters), and flanges are all cylinders. Part Cylinder gives
you a standalone solid that you can subtract from a box (making a through-hole)
or fuse with another solid (adding a boss).

Setting **Angle** to less than 360° produces a cylindrical sector (pie slice),
useful for ventilation ribs or arc-shaped parts.

---

## When to use it

- Quick shaft or boss outside a Part Design Body.
- A through-hole cutter: make a cylinder the same diameter as the hole, then
  Part Cut it from the base solid.
- A partial arc shell when combined with Thickness.

## When NOT to use it

- **Inside a Part Design Body** — use Additive/Subtractive Cylinder.
- **Hollow cylinder** — use [Tube](tube.md) which has both inner and outer radius.

---

## Step-by-step walkthrough

1. **Part → Primitives → Cylinder.**
2. A 2 mm radius, 10 mm tall cylinder appears at the origin.
3. In the **Data** panel, set **Radius** and **Height** to your values.
4. Leave **Angle** at 360° for a full cylinder, or reduce it for a sector.
5. Adjust **Placement** to position the cylinder axis where needed.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius | Distance | 2 mm | Outer radius of the cylinder |
| Height | Distance | 10 mm | Axial length along Z |
| Angle | Angle | 360° | Angular sweep of the cross section (1°–360°) |
| Placement | Placement | Identity | Position and orientation |

> **Note:** The cylinder axis is always along the local Z axis. Rotate
> Placement to reorient it to X or Y.

---

## Common mistakes and pitfalls

!!! warning "Cylinder axis is Z — not the axis you expect"
    The cylinder's axis runs along the local Z axis of its Placement. To create
    a horizontal shaft, rotate the Placement 90° around X or Y.

!!! warning "Radius, not diameter"
    FreeCAD uses radius here. If your drawing specifies ⌀20 mm, enter 10 mm.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Full cylinder
cyl = doc.addObject("Part::Cylinder", "Shaft")
cyl.Radius = 5.0     # mm — 10 mm diameter shaft
cyl.Height = 50.0    # mm
cyl.Angle  = 360.0   # degrees — full circle

doc.recompute()
print(f"Volume: {cyl.Shape.Volume:.2f} mm³")

# Reorient to lie along X axis
import math
cyl.Placement = App.Placement(
    App.Vector(0, 0, 0),
    App.Rotation(App.Vector(0, 1, 0), 90)  # rotate 90° around Y
)
doc.recompute()
```

### Partial cylinder (sector)

```python
sector = doc.addObject("Part::Cylinder", "Sector")
sector.Radius = 20.0
sector.Height = 5.0
sector.Angle  = 90.0   # quarter-circle wedge
doc.recompute()
```

---

## See also

- [Tube](tube.md) — hollow cylinder with inner and outer radius
- [Cone](cone.md) — tapered cylinder variant
- [Boolean Cut](boolean-cut.md) — use a cylinder as a hole cutter
