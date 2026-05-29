# Material for Fluid

> **In one sentence:** Assign density, viscosity, and thermal properties to
> a fluid domain for Elmer flow or conjugate heat transfer analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Materials → Material for Fluid

Creates a fluid material object for use with the Elmer [Flow Equation](equation-flow.md)
(Navier-Stokes) or [Heat Equation](equation-heat.md) (conjugate heat transfer).

---

## Key fluid properties

| Property | Symbol | Unit | Water at 20 °C |
|----------|--------|------|----------------|
| Density | ρ | kg/m³ | 998 |
| Dynamic Viscosity | μ | Pa·s | 0.001 |
| Kinematic Viscosity | ν | m²/s | 1.0×10⁻⁶ |
| Thermal Conductivity | λ | W/(m·K) | 0.598 |
| Specific Heat | c | J/(kg·K) | 4182 |

---

## See also

- [Material for Solid](material-solid.md) — material for solid structural/thermal bodies
- [Equation: Flow](equation-flow.md) — Elmer Navier-Stokes equation
- [FEM Workbench](../index.md) — workbench overview
