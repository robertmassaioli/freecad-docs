# Add Cosmetic Line Through 2 Points

> **In one sentence:** Draws a straight cosmetic line between two selected
> vertices in any line style — for pitch circle diameters, reference
> construction lines, or drawing-only geometry.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Add Lines → Add Cosmetic Line Through 2 Points

Unlike a centerline, the style is not automatically set — you choose any
available line type (solid, dashed, dotted, phantom, etc.). The line exists
only on the drawing, not in the 3-D model.

---

## Common uses

- Pitch circle diameter (imaginary circle on which bolt holes sit, drawn as
  a centerline)
- Reference construction lines for alignment or layout
- Any line that must appear on the drawing but is not a model edge

---

## Common mistakes and pitfalls

!!! warning "Cosmetic lines cannot be dimensioned directly"
    Dimensions cannot be linked to cosmetic lines in the same way as model
    edges. To dimension to a cosmetic position, add a Cosmetic Vertex and
    dimension to that vertex.

---

## See also

- [Add Face Centerline](centerline-face.md) — automatic centerline for faces
- [Add Cosmetic Vertex](cosmetic-vertex.md) — add a dimensionable point
- [TechDraw Workbench](../index.md) — workbench overview
