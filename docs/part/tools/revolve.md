# Revolve

> **In one sentence:** Spin a 2-D profile around an axis to create a solid of
> revolution — the Part workbench equivalent of a lathe operation.

---

## Overview

**Workbench:** Part · **Menu:** Part → Revolve  
**Toolbar:** Part Modeling · **Shortcut:** none

Revolve takes a profile (face, wire, or edge) and sweeps it around a defined
axis by a specified angle. A full 360° sweep produces a closed solid of
revolution; a partial sweep produces a sector.

---

## Intuition

A lathe creates solids of revolution by spinning a profile around the spindle
axis. Part Revolve is the digital equivalent: define the profile on one side of
the axis, revolve it, and FreeCAD produces the rotational solid.

Common shapes created with Revolve:
- **Turned shafts and bushings** — revolve an L-shaped profile around the shaft centreline.
- **Bowls and vases** — revolve an open curve.
- **Flanges** — revolve a rectangular profile with an offset from the axis.
- **Partial sectors** — revolve < 360° for a segment.

---

## When to use it

- Any axially symmetric part: shaft, bushing, flange, bottle, bowl.
- Creating a partial solid of revolution (sector, half-revolution).

## When NOT to use it

- **Profile crosses the axis** — OCCT may produce self-intersecting geometry.
  Keep the profile entirely on one side of the axis.
- **Inside a Part Design Body** — use Revolution (Part Design).

---

## Step-by-step walkthrough

1. **Create the profile** — a Sketcher sketch or a wire on one side of the
   intended rotation axis.
2. **Select the profile.**
3. **Part → Revolve.**
4. In the dialog:
   - Set **Axis** (default: Y axis).
   - Set **Angle** (360° for full revolution).
   - Check **Create Solid**.
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Axis | Vector | Y axis (0,1,0) | Direction of the revolution axis (passes through Placement origin) |
| Angle | Angle | 360° | Angular sweep |
| Create Solid | Bool | Yes | Solid vs surface |
| Symmetric | Bool | No | Revolve ±Angle/2 symmetrically |

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# L-shaped profile for a shaft shoulder
# (points defining the cross-section on the XZ plane, rotated around Z)
import math

profile_points = [
    App.Vector(5, 0, 0),   # inner bottom
    App.Vector(10, 0, 0),  # outer bottom
    App.Vector(10, 0, 20), # outer top
    App.Vector(7, 0, 20),  # shoulder step
    App.Vector(7, 0, 30),  # top
    App.Vector(5, 0, 30),  # inner top
    App.Vector(5, 0, 0),   # back to start
]
edges = [Part.makeLine(profile_points[i], profile_points[i+1])
         for i in range(len(profile_points)-1)]
wire = Part.Wire(edges)
face = Part.Face(wire)

# Revolve around Z axis
axis = App.Vector(0, 0, 1)
solid = face.revolve(App.Vector(0, 0, 0), axis, 360)  # full revolution

feat = doc.addObject("Part::Feature", "Shaft")
feat.Shape = solid
doc.recompute()
print(f"Volume: {feat.Shape.Volume:.2f} mm³")
```

---

## Common mistakes and pitfalls

!!! warning "Profile must not cross the rotation axis"
    If the profile crosses the axis, OCCT produces self-intersecting geometry
    that is invalid for further operations. Keep the entire profile on one side.

!!! warning "Axis direction vs. axis position"
    The revolution axis passes through the **Placement origin** of the profile
    object in the direction specified. Ensure the profile is positioned
    correctly relative to that axis.

---

## See also

- [Extrude](extrude.md) — linear extrusion
- [Loft](loft.md) — blend between multiple profiles
- [Sweep](sweep.md) — sweep a profile along an arbitrary path
