# VTK Calculator

> **In one sentence:** Defines a custom scalar or vector field using an
> expression built from existing result fields and mathematical operations.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Calculator

Creates a new result field by evaluating a user-defined expression over
every node in the mesh. The new field can then be displayed, contoured,
or used as input to further filters — just like any solver-computed field.

---

## Example expressions

| Expression | Meaning |
|------------|---------|
| `Von Mises Stress / 250` | Factor of safety (yield stress = 250 MPa) |
| `Temperature - 20` | Temperature rise above 20 °C ambient |
| `sqrt(Displacement_x^2 + Displacement_y^2)` | 2-D in-plane displacement magnitude |
| `250 - Von Mises Stress` | Safety margin to yield |

---

## Use cases

- **Factor of safety maps:** Divide yield stress by Von Mises stress to colour
  the mesh by safety margin rather than raw stress.
- **Temperature rise:** Subtract ambient temperature to show ΔT instead of
  absolute temperature.
- **Custom vector magnitude:** Build a 2-D magnitude from selected components
  when the full 3-D magnitude includes an irrelevant out-of-plane component.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Contours](vtk-contours.md) — draw iso-surfaces of the custom field
- [Clip Scalar](vtk-clip-scalar.md) — clip by the custom field value
- [FEM Workbench](../index.md) — workbench overview
