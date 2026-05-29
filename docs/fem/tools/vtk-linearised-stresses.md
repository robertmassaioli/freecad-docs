# VTK Linearised Stresses

> **In one sentence:** Decomposes the through-thickness stress distribution into
> membrane, bending, and peak components per ASME VIII / EN 13445 methodology.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Linearised Stresses

Computes the linearised stress components through the thickness of a wall or
section from a [Data Along Line](vtk-data-along-line.md) that traverses the
full wall thickness. Required for pressure vessel design-code stress
categorisation.

---

## Stress components

| Component | Symbol | Description |
|-----------|--------|-------------|
| Membrane stress | P_m or P_L | Average stress through thickness |
| Bending stress | P_b | Linear-varying part about the mid-surface |
| Peak stress | F | Non-linear residual = total − membrane − bending |

These decomposed components map directly to the stress categories in
ASME Boiler & Pressure Vessel Code Section VIII Div. 2 and EN 13445.

---

## Step-by-step

1. Create a [Data Along Line](vtk-data-along-line.md) that runs from one
   free surface to the opposite free surface through the **full wall thickness**.
2. Select the Data Along Line object in the pipeline tree.
3. Add **Linearised Stresses** as a child filter.
4. The tool reports membrane, bending, and peak stress values.

---

## Common mistakes and pitfalls

!!! warning "Line must run full wall thickness"
    Linearised Stresses only gives correct results when the Data Along Line
    runs from one free surface to the opposite free surface through the
    complete wall thickness. A diagonal or partial line gives incorrect
    membrane and bending stress decomposition.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Data Along Line](vtk-data-along-line.md) — required prerequisite
- [FEM Workbench](../index.md) — workbench overview
