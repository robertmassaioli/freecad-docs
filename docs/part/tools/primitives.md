# Primitives Dialog

> **In one sentence:** A single dialog that provides access to all the
> less-common parametric primitive shapes: Plane, Helix, Spiral, Circle,
> Ellipse, Wedge, Vertex, Line, and Regular Polygon.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Create Primitives…  
**Toolbar:** Part Primitives · **Shortcut:** none

The Primitives dialog is a unified creation interface for shapes that do not
have their own toolbar button. Select the desired shape from the dropdown,
fill in the parameters, and click **Create**.

---

## Intuition

Shapes like helices, spirals, and ellipses appear less often than boxes and
cylinders, so they are grouped behind a single dialog rather than cluttering
the toolbar. The dialog allows you to tweak parameters before creating the
shape, unlike the direct primitive buttons which create at defaults and let you
edit afterward.

---

## Available shapes

| Shape | What it creates |
|-------|----------------|
| Plane | Flat rectangular face (not a solid) |
| Box | Rectangular box (also available directly) |
| Cylinder | Right cylinder (also available directly) |
| Cone | Cone or frustum (also available directly) |
| Sphere | Sphere or segment (also available directly) |
| Ellipsoid | Oblate or prolate ellipsoid |
| Torus | Ring shape (also available directly) |
| Prism | Regular-polygon prism |
| Wedge | Trapezoidal box — different top and bottom rectangles |
| Helix | 3-D coil along the Z axis |
| Spiral | 2-D expanding spiral in the XY plane |
| Circle | 2-D circular edge (wire) |
| Ellipse | 2-D elliptical edge |
| Point (Vertex) | Single vertex point |
| Line | Straight edge between two points |
| Regular Polygon | 2-D regular polygon wire |

---

## Shape details

### Plane

A flat rectangular face. Useful as a construction surface or as the base for a
Ruled Surface or as a cutting plane for Section.

| Parameter | Description |
|-----------|-------------|
| Length | X extent |
| Width | Y extent |

### Wedge

A trapezoidal prism with independent bottom and top rectangle dimensions. Good
for ramps, gussets, and dovetail-like profiles.

| Parameter | Description |
|-----------|-------------|
| Xmin, Ymin, Zmin | Corner of the base rectangle |
| Xmax, Ymax, Zmax | Corner of the top rectangle |
| X2min, Z2min, X2max, Z2max | Top face extent (the "small" rectangle) |

### Helix

A 3-D coil used as a sweep spine for springs, threads, and spiral features.

| Parameter | Description |
|-----------|-------------|
| Pitch | Distance between successive turns (mm) |
| Height | Total axial length of the helix |
| Radius | Coil radius |
| Angle | Taper half-angle (0° = cylindrical helix, >0° = conical helix) |
| Left-handed | Handedness toggle |

### Spiral

A 2-D Archimedean spiral in the XY plane. Useful as a sweep path for
watchspring or volute shapes.

| Parameter | Description |
|-----------|-------------|
| Growth | Radial distance gained per revolution |
| Turns | Number of full revolutions |
| Radius | Starting radius |

### Circle

A 2-D circle **edge** (wire), not a solid. Use as a sweep profile or for
Section operations.

| Parameter | Description |
|-----------|-------------|
| Radius | Circle radius |
| Angle1 | Start angle |
| Angle2 | End angle |

### Ellipse

A 2-D ellipse edge.

| Parameter | Description |
|-----------|-------------|
| MajorRadius | Semi-major axis |
| MinorRadius | Semi-minor axis |
| Angle1, Angle2 | Arc extents |

### Regular Polygon

A 2-D closed wire of N equal sides.

| Parameter | Description |
|-----------|-------------|
| Polygon | Number of sides (3 = triangle, 6 = hexagon, …) |
| Circumradius | Radius of the circumscribed circle |

---

## Step-by-step walkthrough

### Creating a helix for a spring sweep

1. **Part → Primitives → Create Primitives…**
2. Select **Helix** from the dropdown.
3. Set **Pitch** = 5 mm (turn spacing), **Height** = 50 mm, **Radius** = 10 mm.
4. Click **Create**, then **Close**.
5. Select the Helix. Then run [Sweep](sweep.md) with a circular profile to
   create a spring solid.

### Creating a wedge (ramp)

1. Open the Primitives dialog.
2. Select **Wedge**.
3. Adjust Xmin/Xmax for the base, X2min/X2max for the top (narrower creates
   a ramp).
4. Click **Create**.

---

## Common mistakes and pitfalls

!!! warning "Circle and Ellipse produce edges, not faces"
    A Part Circle creates a wire, not a solid or face. Use
    [Face from Wires](face-from-wires.md) to turn it into a face, or use it
    directly as a sweep profile.

!!! warning "Helix has no built-in solid — it is a sweep path"
    To make a spring, create a Helix and then use [Sweep](sweep.md) with a
    circle as the profile.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Helix (spring path)
helix = doc.addObject("Part::Helix", "SpringPath")
helix.Pitch  = 5.0    # mm per revolution
helix.Height = 50.0   # mm total
helix.Radius = 10.0   # mm coil radius
doc.recompute()

# Wedge
wedge = doc.addObject("Part::Wedge", "Ramp")
wedge.Xmin  = 0.0
wedge.Ymin  = 0.0
wedge.Zmin  = 0.0
wedge.Xmax  = 20.0
wedge.Ymax  = 10.0
wedge.Zmax  = 30.0
wedge.X2min = 5.0     # top face starts at X=5
wedge.Z2min = 0.0
wedge.X2max = 15.0    # top face ends at X=15
wedge.Z2max = 30.0
doc.recompute()

# Regular polygon (hexagon wire)
poly = doc.addObject("Part::RegularPolygon", "Hex")
poly.Polygon       = 6      # 6 sides
poly.Circumradius  = 10.0   # mm
doc.recompute()
```

---

## See also

- [Cube](cube.md) — dedicated box button
- [Cylinder](cylinder.md) — dedicated cylinder button
- [Sweep](sweep.md) — sweep a profile along a Helix or Spiral
- [Face from Wires](face-from-wires.md) — convert Circle/Ellipse wire to a face
- [Extrude](extrude.md) — extrude a Polygon wire into a prism
