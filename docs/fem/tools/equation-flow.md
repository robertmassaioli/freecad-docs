# Equation: Flow

> **In one sentence:** Elmer equation for incompressible Navier-Stokes fluid
> flow — solves for velocity and pressure fields.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Flow

Solves the incompressible Navier-Stokes equations for steady-state or
transient viscous fluid flow.

---

## Required

- **BCs:** [Flow Velocity Constraint](constraint-flow-velocity.md) on inlets and walls
- **Material:** Density (ρ) and Dynamic Viscosity (μ) — use [Material for Fluid](material-fluid.md)
- **Mesh:** [Gmsh mesh](mesh-gmsh.md) with [named groups](mesh-group.md); [boundary layers](mesh-boundary-layer.md) for accurate wall resolution

---

## Use cases

Internal duct flow, external aerodynamics (low Re), coolant flow in heat
exchangers.

---

## Conjugate heat transfer

Couple the Flow equation with the [Heat Equation](equation-heat.md) for
conjugate heat transfer (CHT) — fluid carries heat into/out of a solid.

---

## See also

- [Equation: Heat](equation-heat.md) — heat equation (coupled with Flow for CHT)
- [Constraint: Flow Velocity](constraint-flow-velocity.md) — inlet/wall BCs
- [Material for Fluid](material-fluid.md) — fluid properties
- [Solver Elmer](solver-elmer.md) — must be added first
- [FEM Workbench](../index.md) — workbench overview
