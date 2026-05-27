# Torus

> **In one sentence:** Create a parametric torus (donut shape) with configurable
> major radius, minor radius, and angular sweeps.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Torus  
**Toolbar:** Part Primitives · **Shortcut:** none

The Torus primitive creates a ring shape defined by sweeping a circle of radius
**Radius2** around an axis at distance **Radius1**. Both radii and all three
angular extents are live parameters.

---

## Intuition

Toruses appear as:
- **O-ring seats** — subtract a partial torus from a face to create a groove.
- **Decorative rings** — full tori for jewellery or product design.
- **Pipe bends** — a partial torus sector captures the swept geometry of a
  curved pipe.

The three angle parameters let you control how much of the torus is created:
- **Angle1** and **Angle2** clip the tube cross-section (making a partial tube
  in the poloidal direction).
- **Angle3** sweeps the tube around the main axis (toroidal direction).

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Distance | 10 mm | Distance from the torus centre to the centre of the tube |
| Radius2 | Distance | 2 mm | Radius of the tube cross-section |
| Angle1 | Angle | −180° | Poloidal start angle |
| Angle2 | Angle | 180° | Poloidal end angle |
| Angle3 | Angle | 360° | Toroidal sweep angle |
| Placement | Placement | Identity | Position and orientation |

### Typical configurations

| Use case | Angle1 | Angle2 | Angle3 |
|----------|--------|--------|--------|
| Full torus | −180° | 180° | 360° |
| 90° pipe bend | −180° | 180° | 90° |
| O-ring groove (half-circle cross-section) | 0° | 180° | 360° |

---

## Step-by-step walkthrough

1. **Part → Primitives → Torus.**
2. A 10 mm major radius, 2 mm tube radius full torus appears.
3. In **Data**, adjust **Radius1** (ring size) and **Radius2** (tube thickness).
4. Use **Angle3** to make a partial arc instead of a full ring.
5. Position with **Placement**.

---

## Common mistakes and pitfalls

!!! warning "Radius1 must be greater than Radius2"
    If Radius2 ≥ Radius1, the tube passes through the axis and creates
    self-intersecting geometry. Keep Radius1 > Radius2 by a comfortable margin.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Full torus
torus = doc.addObject("Part::Torus", "Ring")
torus.Radius1 = 15.0  # ring radius
torus.Radius2 = 3.0   # tube radius

doc.recompute()
print(f"Volume: {torus.Shape.Volume:.2f} mm³")

# 90-degree pipe elbow
elbow = doc.addObject("Part::Torus", "Elbow90")
elbow.Radius1 = 20.0
elbow.Radius2 = 5.0
elbow.Angle3  = 90.0  # 90° sweep

doc.recompute()
```

---

## See also

- [Cylinder](cylinder.md) — straight pipe primitive
- [Tube](tube.md) — hollow cylinder (not curved)
- [Sweep](sweep.md) — sweep a custom cross-section along a curved path
