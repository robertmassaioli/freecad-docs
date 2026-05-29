# Section Print

> **In one sentence:** Define a cross-sectional cut plane and request
> CalculiX to output the resultant forces and moments through it.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Geometrical Analysis Features → Section Print

Defines a cross-sectional cut plane and instructs CalculiX to output the
resultant forces and moments that pass through that cross-section.

---

## When to use it

- Verifying that the resultant force on a cut through a bolt shank equals
  the applied load.
- Extracting the bending moment and shear on a specific beam cross-section.
- Checking section loads for downstream hand calculations.

---

## Common mistakes and pitfalls

!!! warning "Output file format must be configured"
    Section Print adds a request to the CalculiX input file, but the output
    must also be configured in the CalculiX output settings to include section
    forces.

---

## See also

- [Plane Rotation Constraint](constraint-plane-rotation.md) — cyclic symmetry
- [Solver CalculiX](solver-calculix.md) — the solver that processes this
- [FEM Workbench](../index.md) — workbench overview
