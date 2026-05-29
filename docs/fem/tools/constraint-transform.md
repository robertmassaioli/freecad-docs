# Transform Constraint

> **In one sentence:** Apply displacement boundary conditions in a local
> coordinate system rather than the global X/Y/Z frame.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Geometrical Analysis Features → Transform Constraint

Applies displacement boundary conditions in a local coordinate system
(cylindrical or Cartesian) rather than global X/Y/Z. This allows constraints
that are impossible to express cleanly in global coordinates.

---

## When to use it

- **Cylindrical symmetry:** Fix radial displacement on a cylindrical face
  while allowing circumferential and axial displacement.
- **Inclined surface:** Constrain motion normal to an inclined surface (e.g.
  a 30° chamfer must not penetrate a mating part).

---

## Step-by-step

1. Choose **FEM → Model → Geometrical → Transform Constraint**.
2. Select the face to constrain.
3. Set the coordinate system type (Cylindrical or Cartesian) and orientation.
4. Set which components are fixed in the local system.
5. Click **OK**.

---

## See also

- [Displacement Constraint](constraint-displacement.md) — global-axis displacement BC
- [Plane Rotation Constraint](constraint-plane-rotation.md) — cyclic symmetry BC
- [FEM Workbench](../index.md) — workbench overview
