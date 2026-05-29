# Fluid Section for 1-D Flow

> **In one sentence:** Define hydraulic cross-section properties for edges
> modelled as 1-D pipe-flow elements in a CalculiX network flow analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Element Geometry → Fluid Section for 1-D Flow

Defines the hydraulic properties of edges modelled as 1-D pipe-flow elements.
Used with CalculiX's network flow analysis — fluid flowing through a network
of pipes, channels, or orifices — rather than full 3-D CFD.

---

## Properties

| Property | Description |
|----------|-------------|
| Section Type | Pipe, Sluice, Weir, etc. |
| Inner Radius | Inner radius for circular pipe sections |
| Outer Radius | Outer radius (for annular sections) |
| Manning Number | Roughness coefficient for open-channel flow |
| Length | Characteristic length when geometry is not explicit |

---

## When to use 1-D flow

1-D network flow analysis is appropriate when:
- The flow path is simple (pipes, channels, orifices).
- Only pressure drops and flow rates are needed — not detailed 3-D flow fields.
- The full 3-D Elmer Navier-Stokes solver is too expensive.

---

## See also

- [Beam Cross Section](beam-cross-section.md) — cross-section for structural beam elements
- [Solver CalculiX](solver-calculix.md) — the solver that handles 1-D network flow
- [FEM Workbench](../index.md) — workbench overview
