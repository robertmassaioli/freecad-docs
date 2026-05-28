# Electromagnetic, Fluid, and Geometrical Constraints

> **In one sentence:** These boundary conditions extend FEM beyond solid
> mechanics — prescribing electric potentials and current for EM analysis,
> flow velocities and pressures for fluid simulations, and geometrical
> constraints that shape how the solver treats specific regions.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Electromagnetic / Fluid / Geometrical Analysis Features

| Group | Tool | Description |
|-------|------|-------------|
| **Electromagnetic** | [Electrostatic Potential](#electrostatic-potential) | Prescribe electric potential (V) on a boundary |
| | [Current Density](#current-density) | Prescribe current density (A/m²) |
| | [Magnetization](#magnetization) | Apply magnetization vector |
| | [Electric Charge Density](#electric-charge-density) | Prescribe surface or volumetric charge density |
| **Fluid** | [Initial Flow Velocity](#initial-flow-velocity) | Starting velocity for transient fluid flow |
| | [Initial Pressure](#initial-pressure) | Starting pressure for transient fluid flow |
| | [Flow Velocity Constraint](#flow-velocity-constraint) | Prescribe velocity on inlet/wall faces |
| **Geometrical** | [Plane Rotation Constraint](#plane-rotation-constraint) | Constrain nodes to a plane (cyclic symmetry) |
| | [Section Print](#section-print) | Output reaction forces on a cross-section |
| | [Transform Constraint](#transform-constraint) | Apply BCs in a local coordinate system |

---

## Electromagnetic Boundary Conditions

These constraints are used with the **Elmer** solver and its electrostatic,
electromagnetic, or magnetodynamic equations.

### Electrostatic Potential

**Menu:** FEM → Model → Electromagnetic BCs → Electrostatic Potential

Prescribes the electric potential (voltage, in volts) on a face, edge, or
vertex. This is the electromagnetic equivalent of the temperature constraint
in thermal analysis — it fixes the value of the unknown field (potential)
at a boundary.

#### When to use it

- **Anode/cathode:** Set the potential on the + electrode to the supply
  voltage (e.g. 100 V) and the − electrode to 0 V (ground).
- **Grounded conductor:** Set a face to 0 V.
- **Floating conductor at known potential:** Set to a fixed voltage.

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Potential | Electric potential (V) |
| References | Face, edge, or vertex |
| Potential is constant | Fix to the same potential regardless of current |

#### Solver

Used with:
- Elmer + Electrostatic Equation
- Elmer + Magnetodynamic Equation (for AC fields)
- CalculiX electromagnetic (electrostatic type)

#### Python API

```python
import ObjectsFem

pot = ObjectsFem.makeConstraintElectrostaticPotential(doc, "Anode")
pot.Potential = 100.0   # V
pot.References = [(doc.getObject("Body"), "Face2")]
analysis.addObject(pot)
doc.recompute()
```

---

### Current Density

**Menu:** FEM → Model → Electromagnetic BCs → Current Density

Prescribes the electric current density (A/m²) as a Neumann boundary
condition on a face — the electromagnetic equivalent of a heat flux load.
Use this when the current flowing through a surface is known rather than the
potential.

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Current density (x, y, z) | Current density vector components (A/m²) |
| References | Face to carry the prescribed current density |

---

### Magnetization

**Menu:** FEM → Model → Electromagnetic BCs → Magnetization

Applies a magnetization vector to a region of the model, representing a
permanent magnet or a pre-magnetised body. Used with the Elmer
Magnetodynamic or Demagnetization equations.

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Magnetization vector (x, y, z) | Magnetization components (A/m) |
| References | Body or face to magnetise |

---

### Electric Charge Density

**Menu:** FEM → Model → Electromagnetic BCs → Electric Charge Density

Prescribes a surface charge density (C/m²) or volumetric charge density
(C/m³) on a boundary. Used with the Elmer Electrostatic equation to model
charged dielectric surfaces or space charge distributions.

---

## Fluid Boundary Conditions

These constraints are used with the **Elmer** solver and its Flow equation
(Navier-Stokes for incompressible flow).

### Initial Flow Velocity

**Menu:** FEM → Model → Fluid BCs → Initial Flow Velocity

Sets the uniform starting velocity field for a **transient** (time-dependent)
fluid flow analysis. At t = 0, every point in the fluid domain has this velocity.

For steady-state flow analysis, the initial velocity is a starting guess for
the iterative solver — a good initial guess (close to the expected solution)
speeds up convergence.

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Velocity (x, y, z) | Initial velocity vector (m/s) |
| Enable component | Toggle each velocity component independently |

---

### Initial Pressure

**Menu:** FEM → Model → Fluid BCs → Initial Pressure

Sets the starting pressure field for a transient fluid flow analysis. For
an incompressible flow with a known inlet pressure, set Initial Pressure to
match the inlet BC to speed up convergence.

---

### Flow Velocity Constraint

**Menu:** FEM → Model → Fluid BCs → Flow Velocity Constraint

Prescribes the fluid velocity on selected faces — the primary inlet/outlet
and wall boundary condition for Navier-Stokes analysis.

#### Common configurations

| Face type | Velocity prescription |
|-----------|----------------------|
| Inlet | Set all velocity components (uniform or profile) |
| Wall (no-slip) | All components = 0 (fluid cannot penetrate or slide on the wall) |
| Slip wall | Normal component = 0, tangential components free |
| Outlet | Often left unconstrained (natural outflow BC) |
| Symmetry plane | Normal component = 0 |

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Velocity x | X-component of velocity (m/s) or free |
| Velocity y | Y-component |
| Velocity z | Z-component |
| Normalise | Normalise the velocity to a unit inlet profile |

---

## Geometrical Analysis Features

### Plane Rotation Constraint

**Menu:** FEM → Model → Geometrical Analysis Features → Plane Rotation Constraint

Constrains all nodes on a face to remain in the plane of that face. This
enforces **cyclic symmetry** or **anti-symmetry** conditions — a standard
technique to model only one sector of a rotationally periodic structure
(turbine disc, bolt circle, fan blade).

#### How cyclic symmetry works

Instead of meshing the full 360° disc, mesh one sector (e.g. 1 blade out
of 24, = 15°). Apply Plane Rotation to the two cut faces to force them to
deform as mirror images of each other. The solver then represents the full
disc solution at 1/24th the cost.

---

### Section Print

**Menu:** FEM → Model → Geometrical Analysis Features → Section Print

Defines a cross-sectional cut plane and instructs CalculiX to output the
resultant forces and moments that pass through that cross-section. Used to
compute reaction forces or section loads without needing to post-process the
stress field manually.

#### When to use it

- Checking that the resultant force on a cut through a bolt shank equals the
  applied load (verification).
- Extracting the bending moment and shear on a specific cross-section of a
  beam for downstream calculations.

---

### Transform Constraint

**Menu:** FEM → Model → Geometrical Analysis Features → Transform Constraint

Applies displacement boundary conditions in a **local coordinate system**
rather than the global X/Y/Z frame. Supports cylindrical and Cartesian local
systems.

#### When to use it

- **Cylindrical symmetry:** Fix the radial displacement on a cylindrical
  face but allow circumferential and axial displacement — impossible to
  express in global XYZ but trivial in a local cylindrical frame.
- **Inclined surface constraint:** Constrain motion normal to an inclined
  surface (e.g. a 30° chamfer must not penetrate a mating part).

#### Step-by-step

1. Choose **FEM → Model → Geometrical → Transform Constraint**.
2. Select the face to constrain.
3. Set the coordinate system type (Cylindrical or Cartesian) and the
   orientation.
4. Set which components are fixed in the local system.
5. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Electromagnetic BCs require the correct Elmer equation"
    Electrostatic Potential does nothing if the analysis uses a thermal or
    mechanical equation. You must add the matching Elmer equation (Electrostatic,
    Magnetodynamic, etc.) to the analysis alongside the boundary conditions.

!!! warning "Flow inlet velocity must be physically consistent"
    Setting a high inlet velocity without a matching outlet or pressure BC
    may cause the Navier-Stokes solver to diverge. For incompressible flow,
    the velocity field must satisfy continuity (∇·v = 0). Ensure the inlet
    flow equals the outlet flow for steady-state analysis.

!!! warning "Plane Rotation for cyclic symmetry requires matching meshes"
    The two sector cut faces must have the same mesh pattern (same node
    positions on both faces in the undeformed state) for the cyclic
    symmetry to work correctly. Use Gmsh's periodic meshing option.

!!! warning "Section Print output must be requested in CalculiX settings"
    The Section Print tool adds a request to the CalculiX input file, but the
    output file format must also be set correctly. Check the CalculiX output
    settings (Output → Output Variables) to ensure section forces are included.

---

## See also

- [Materials](materials.md) — dielectric constants and fluid viscosity for EM/fluid analysis
- Solver Elmer — electrostatic, flow, and magnetodynamic equations
- Solver CalculiX — cyclic symmetry, section forces
- Solvers and Equations — which equations pair with which BCs
