# Plane Rotation Constraint

> **In one sentence:** Constrain all nodes on a face to remain in the plane
> of that face — used for cyclic symmetry boundary conditions.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Geometrical Analysis Features → Plane Rotation Constraint

Constrains all nodes on a selected face to remain in the plane of that face.
The primary use is **cyclic symmetry** — modelling only one periodic sector
of a rotationally periodic structure (turbine disc, bolt circle, fan blade).

---

## How cyclic symmetry works

Instead of meshing the full 360° disc, mesh one sector (e.g. 1/24th = 15°).
Apply Plane Rotation to both cut faces to enforce that they deform as mirror
images. The solver represents the full disc solution at 1/24th the cost.

---

## Common mistakes and pitfalls

!!! warning "Requires matching mesh patterns on both cut faces"
    The two sector cut faces must have the same mesh pattern (same node
    positions) for cyclic symmetry to work correctly. Use Gmsh's periodic
    meshing option to enforce this.

---

## See also

- [Transform Constraint](constraint-transform.md) — apply BCs in a local coordinate system
- [Section Print](section-print.md) — output section forces
- [FEM Workbench](../index.md) — workbench overview
