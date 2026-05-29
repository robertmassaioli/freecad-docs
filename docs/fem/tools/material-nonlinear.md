# Nonlinear Mechanical Material

> **In one sentence:** Add a plasticity model on top of an existing solid
> material to enable post-yield deformation in CalculiX nonlinear analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Materials → Nonlinear Mechanical Material

Adds a plasticity model (`FemMaterialMechanicalNonlinear`) on top of an
existing [Material for Solid](material-solid.md). This allows the material
to yield — deform permanently when stress exceeds the yield point — rather
than behaving elastically at all stresses.

---

## When to use it

- Stresses are expected to exceed the yield strength.
- You need to predict permanent deformation (set) after unloading.
- Required for CalculiX nonlinear static analysis.

---

## Plasticity models

| Model | Description |
|-------|-------------|
| Isotropic hardening | Yield surface expands uniformly; suitable for most metals |
| Kinematic hardening | Yield surface translates; better for cyclic loading |

---

## Step-by-step

1. A [Material for Solid](material-solid.md) must already exist in the analysis.
2. Select the solid material object in the model tree.
3. Choose **FEM → Model → Materials → Nonlinear Mechanical Material**.
4. Set **Yield Strength** and **Hardening Slope**.
5. Set the CalculiX solver's **Geometrical Nonlinearity** to `On`.

---

## See also

- [Material for Solid](material-solid.md) — the base solid material this extends
- [Solver CalculiX](solver-calculix.md) — must be in nonlinear static mode
- [FEM Workbench](../index.md) — workbench overview
