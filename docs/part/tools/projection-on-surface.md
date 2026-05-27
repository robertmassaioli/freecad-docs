# Projection on Surface

> **In one sentence:** Project wires, edges, or faces from a source object
> onto a target surface, following a linear projection direction.

---

## Overview

**Workbench:** Part · **Menu:** Part → Projection on Surface  
**Toolbar:** Part Modeling · **Shortcut:** none

Projection on Surface takes geometry (wires, edges, or faces) and projects it
onto a specified target surface along a given direction. The result is a set of
edges or faces that lie on the target surface's geometry.

---

## Intuition

Projecting artwork onto a curved surface is the visual metaphor: a logo drawn
on a flat plane is projected like a slide projector onto the side of a product,
and the projection wraps around the curves of the surface. The logo geometry
follows the contours of the target.

In FreeCAD, Projection on Surface is used for:
- **Embossed labels and markings** — project a wire profile onto a curved
  surface, then extrude it to create raised text or logos.
- **Curved wires** — project a straight wire path onto a curved surface to
  create a wire that follows the surface.
- **Mould engraving** — project a flat design onto a mould surface.

---

## When to use it

- You have a flat wire or face design and need it to lie on a curved surface.
- Creating embossed or engraved features on a non-planar face.
- Generating a surface-following guide wire for a sweep on a complex surface.

---

## Step-by-step walkthrough

1. **Part → Projection on Surface.**
2. In the dialog:
   - Set the **Target Surface** (the surface to project onto).
   - Set the **Source shapes** (wires/faces to project).
   - Set the **Direction** of projection (e.g., Z for projecting downward).
3. Choose what to output: **Faces** (projected filled areas), **Edges**
   (projected wire outlines), or **Wire** (projected wire path only).
4. Click **OK**.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Surface | Part face | Target surface to project onto |
| Wire/Edge | List | Source wires or edges to project |
| Direction | Vector | Projection direction (default: surface normal or Z axis) |
| Height | Distance | Extrusion height of projected faces (if outputting faces) |

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Create a curved target surface (top face of a cylinder)
cyl = doc.addObject("Part::Cylinder", "Target")
cyl.Radius = 30; cyl.Height = 20
doc.recompute()

# Create source wire (a circle in the XY plane)
import Part
src_circle = Part.makeCircle(10, App.Vector(0, 0, 25), App.Vector(0, 0, 1))
src_wire = Part.Wire([src_circle])
src_feat = doc.addObject("Part::Feature", "SourceWire")
src_feat.Shape = src_wire
doc.recompute()

# Use Part::Projection interactively — set up parameters in the dialog
proj = doc.addObject("Part::Projection", "Projected")
proj.Source = src_feat
proj.Surface = cyl
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Projection direction must intersect the target"
    If the projection direction is parallel to the target surface at the source
    point, the projection fails. Ensure the direction vector points into the
    surface.

!!! warning "Source geometry must be within the projection footprint"
    Source wires outside the boundary of the target surface produce partial or
    no projection. Position the source within the target surface bounds.

---

## See also

- [Extrude](extrude.md) — extrude the projected wire into a solid boss/groove
- [Offset 3D](offset-3d.md) — offset the target surface for clearance
- [Section](section.md) — find the intersection wire between two shapes
