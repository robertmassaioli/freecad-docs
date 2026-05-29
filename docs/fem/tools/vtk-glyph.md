# VTK Glyph

> **In one sentence:** Places an arrow or cone at each mesh node, scaled and
> oriented by a vector result field, to visualise direction and magnitude.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Glyph

Renders a glyph (arrow, cone, or sphere) at each node position. The glyph
orientation follows the vector field direction and the glyph size is
proportional to the magnitude. Used to visualise displacement direction,
heat flux paths, or velocity vectors in a flow field.

---

## Use cases

- **Displacement vectors:** See how different parts of a structure move under
  load, not just the magnitude.
- **Heat flux direction:** Identify the dominant heat flow paths through a
  component.
- **Flow velocity:** Visualise recirculation zones and flow separation in a
  fluid domain.

---

## Tips

The glyph density can be reduced (show every Nth node) for clarity in large
meshes. A uniform glyph scale is usually more readable than per-node scaling
when the magnitudes vary by orders of magnitude.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Warp by Vector](vtk-warp.md) — deform the mesh shape instead of drawing arrows
- [Contours](vtk-contours.md) — visualise scalar fields as iso-surfaces
- [FEM Workbench](../index.md) — workbench overview
