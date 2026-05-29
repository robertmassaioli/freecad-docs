# Spring Constraint

> **In one sentence:** Attach a linear elastic spring (F = k × d) to a face,
> edge, or vertex to model compliant supports such as rubber mounts or soil.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions → Spring Constraint

Adds a linear spring to a face, edge, or vertex. The spring exerts a
restoring force proportional to displacement: F = k × d.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Normal stiffness | Spring stiffness in the face-normal direction (N/mm) |
| Tangential stiffness | Spring stiffness parallel to the face (N/mm) |
| Reference edge | Optional: sets the spring direction |

---

## Use cases

- Simulating a flexible foundation (soil, rubber mount, compliant support).
- Modelling a structure supported by springs rather than rigidly fixed.
- Representing the stiffness of adjacent structure without meshing it
  (foundation stiffness method).

---

## See also

- [Fixed Constraint](constraint-fixed.md) — fully rigid support
- [Displacement Constraint](constraint-displacement.md) — prescribed displacement
- [FEM Workbench](../index.md) — workbench overview
