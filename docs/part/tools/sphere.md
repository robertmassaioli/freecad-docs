# Sphere

> **In one sentence:** Create a parametric sphere (or spherical segment) whose
> radius and angular extents can be changed at any time.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Sphere  
**Toolbar:** Part Primitives · **Shortcut:** none

The Sphere primitive creates a full sphere or a portion of a sphere defined by
two latitude angles and a longitude sweep. The radius is a live parameter.

---

## Intuition

Full spheres appear less often than cylinders in mechanical work but are common
in mould-making, ergonomic grips, optical components, and ball joints. The
partial sphere parameters (Angle1, Angle2, Angle3) let you create caps,
segments, and sectors without any Boolean operations.

---

## When to use it

- Ball or dome shapes.
- Convex end caps for pressure vessels.
- Decorative hemispherical features.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius | Distance | 5 mm | Sphere radius |
| Angle1 | Angle | −90° | Start latitude (−90° = south pole) |
| Angle2 | Angle | 90° | End latitude (90° = north pole) |
| Angle3 | Angle | 360° | Longitude sweep (360° = full sphere) |
| Placement | Placement | Identity | Position and orientation |

### Latitude angles (Angle1, Angle2)

- `Angle1 = −90°`, `Angle2 = 90°` → full sphere  
- `Angle1 = 0°`, `Angle2 = 90°` → upper hemisphere  
- `Angle1 = −90°`, `Angle2 = 0°` → lower hemisphere  
- Any range in between → a spherical zone (band)

### Longitude angle (Angle3)

- `360°` → full revolution (no cut face)  
- `180°` → half-sphere sector (one flat face on the cut plane)

---

## Step-by-step walkthrough

1. **Part → Primitives → Sphere.**
2. A 5 mm radius sphere appears at the origin.
3. In the **Data** panel, set **Radius** to your value.
4. Leave Angle1/Angle2/Angle3 at defaults for a full sphere.
5. Adjust **Placement** as needed.

---

## Common mistakes and pitfalls

!!! warning "Centre at origin, not tangent to plane"
    The sphere centre is at the Placement origin. To rest the sphere on the XY
    plane (touching it at the south pole), set Placement Z to +Radius.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Full sphere
sph = doc.addObject("Part::Sphere", "Ball")
sph.Radius = 10.0   # mm

doc.recompute()
print(f"Volume: {sph.Shape.Volume:.2f} mm³")   # ≈ 4188.79

# Upper hemisphere
hemi = doc.addObject("Part::Sphere", "Hemisphere")
hemi.Radius = 10.0
hemi.Angle1 = 0.0    # start at equator
hemi.Angle2 = 90.0   # end at north pole

doc.recompute()
print(f"Hemisphere volume: {hemi.Shape.Volume:.2f} mm³")   # ≈ 2094.40
```

---

## See also

- [Cylinder](cylinder.md) — cylindrical primitive
- [Torus](torus.md) — ring-shaped primitive
- [Primitives dialog](primitives.md) — less common shapes (ellipse, wedge, …)
