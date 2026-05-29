# VTK Contours

> **In one sentence:** Draws iso-contour surfaces (3-D) or lines (on a cut
> surface) at selected field values throughout the result volume.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Contours

Generates iso-contour surfaces or lines where the scalar field equals a
specific value. Contours show the geometry of constant-field surfaces
throughout the mesh volume — isothermal surfaces for temperature, isobaric
surfaces for pressure, or yield-stress contours for Von Mises stress.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Field | Scalar field to contour |
| Number of contours | Evenly-spaced contours between Min and Max |
| Contour values | Specific values to draw (manual entry) |

---

## Use cases

- **Thermal analysis:** Isothermal surfaces show the exact shape of temperature
  zones inside a component.
- **Pressure vessel:** Iso-stress contours at 0.6σ_yield and σ_yield mark the
  onset of plasticity.
- **Flow analysis:** Isovelocity contours show flow separation boundaries.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Glyph](vtk-glyph.md) — visualise vector fields with arrows
- [Calculator](vtk-calculator.md) — create a custom field to contour
- [FEM Workbench](../index.md) — workbench overview
