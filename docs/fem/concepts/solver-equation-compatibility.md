# Solver and Equation Compatibility

> **In one sentence:** FreeCAD FEM supports four solvers, each covering a
> different subset of physics — knowing which solver handles which analysis
> type, and which boundary conditions and equations are required, prevents the
> most common "solver won't run" errors.

---

## The four solvers

| Solver | Executable | Physics covered | Best for |
|--------|-----------|-----------------|---------|
| **CalculiX** | `ccx` | Structural, thermal, thermo-mechanical, buckling, frequency, electromagnetic | Most structural and thermal work |
| **Elmer** | `ElmerSolver` | Electrostatic, electromagnetic, fluid flow, heat, structural deformation | Multiphysics, EM, flow |
| **Mystran** | `mystran` | Linear static structural only | Quick linear checks (NASTRAN-format) |
| **Z88** | `z88r` | Linear static structural only | Quick linear checks (alternative) |

Each solver requires its executable to be installed separately and the path
configured in **Edit → Preferences → FEM → [SolverName]**.

---

## CalculiX — analysis type matrix

CalculiX is controlled by a single **Analysis Type** property. All required
setup (mesh, materials, BCs) is the same structure regardless of type — only
the result outputs differ.

| Analysis Type | Solves | Required BCs | Optional |
|---------------|--------|--------------|---------|
| `static` | Linear/nonlinear deformation and stress | [Fixed Constraint](../tools/constraint-fixed.md) + at least one load | [Spring](../tools/constraint-spring.md), [Contact](../tools/constraint-contact.md) |
| `frequency` | Natural frequencies and mode shapes (modal) | [Fixed Constraint](../tools/constraint-fixed.md) | No loads needed — loads are ignored |
| `thermomech` | Temperature distribution and thermal stress | [Temperature Constraint](../tools/constraint-temperature.md) + [Heat Flux](../tools/load-heat-flux.md) or [Body Heat Source](../tools/load-body-heat-source.md) | Mechanical loads for coupled thermo-mechanical |
| `buckling` | Critical load multiplier (linear buckling) | Same as static | — |
| `check` | Mesh quality check only | None required | — |
| `electromagnetic` | Electrostatic field | [Electrostatic Potential](../tools/constraint-electrostatic-potential.md) | [Electric Charge](../tools/constraint-electric-charge.md) |

!!! warning "Frequency analysis ignores all loads"
    Modal analysis computes natural vibration modes of the structure.
    Applied forces, pressures, and gravity loads are not used. If you want
    stress at a specific mode, run a separate static analysis.

!!! warning "Geometrical Nonlinearity must be enabled for large-deformation problems"
    For problems with displacements greater than ~5% of the part dimensions,
    enable `GeometricalNonlinearity = nonlinear` on the CalculiX solver object.
    The default `linear` setting uses a small-strain formulation.

---

## Elmer — equation-based physics

Elmer has no built-in analysis type. You define the physics by adding
**equation objects** to the Analysis container. Each equation object adds one
governing PDE to the solve.

| Equation | Physics | Required BCs | Required material properties |
|----------|---------|--------------|------------------------------|
| [Elasticity](../tools/equation-elasticity.md) | Linear structural (small strain) | [Displacement](../tools/constraint-displacement.md) | Young's modulus, Poisson ratio, density |
| [Deformation](../tools/equation-deformation.md) | Nonlinear structural (large strain) | [Displacement](../tools/constraint-displacement.md) | Young's modulus, Poisson ratio, density |
| [Heat](../tools/equation-heat.md) | Heat conduction/convection | [Temperature](../tools/constraint-temperature.md) and/or [Heat Flux](../tools/load-heat-flux.md) | Thermal conductivity λ, specific heat c, density ρ |
| [Flow](../tools/equation-flow.md) | Navier-Stokes fluid flow | [Flow Velocity](../tools/constraint-flow-velocity.md) on inlet/wall | Viscosity, density |
| [Electrostatic](../tools/equation-electrostatic.md) | Electric potential ∇·(ε∇V) = −ρ | [Electrostatic Potential](../tools/constraint-electrostatic-potential.md) | Permittivity ε |
| [Static Current](../tools/equation-static-current.md) | DC conduction ∇·(σ∇V) = 0 | [Electrostatic Potential](../tools/constraint-electrostatic-potential.md) on terminals | Conductivity σ |
| [Magnetodynamic](../tools/equation-magnetodynamic.md) | Time-harmonic (AC) EM fields | [Magnetization](../tools/constraint-magnetization.md) or [Current Density](../tools/constraint-current-density.md) | Permeability, conductivity |
| [Magnetodynamic 2D](../tools/equation-magnetodynamic-2d.md) | 2-D planar/axisymmetric EM | Same as Magnetodynamic | Same as Magnetodynamic |
| [Electricforce](../tools/equation-electricforce.md) | Electrostatic body force | Requires Electrostatic to be solved first | — |
| [Flux](../tools/equation-flux.md) | Heat or electric flux (post-processing) | Requires Heat or Electrostatic to be solved first | — |

### Elmer multiphysics combinations

Elmer's power lies in coupling multiple equations in one solve:

| Combination | Use case |
|-------------|---------|
| Flow + Heat | Conjugate heat transfer (fluid cooling a solid) |
| Electrostatic + Electricforce + Elasticity | Electromechanical actuator (MEMS) |
| Magnetodynamic + Heat | Induction heating (Joule heating in conductor) |
| Elasticity + Heat | Thermal stress (uncoupled) |

Add all required equations to the same Analysis. Elmer solves them
simultaneously (or sequentially for weakly coupled systems).

---

## Mystran

Linear static structural only. Equivalent to CalculiX `static` with
`GeometricalNonlinearity = linear`. Use when CalculiX is not installed or
when a NASTRAN-format workflow is required.

**Required:** [Fixed Constraint](../tools/constraint-fixed.md) + at least one mechanical load.

---

## Z88

Linear static structural only. Equivalent to CalculiX `static` with
`GeometricalNonlinearity = linear`. Alternative lightweight solver.

**Required:** [Fixed Constraint](../tools/constraint-fixed.md) + at least one mechanical load.

---

## Choosing the right solver

| Scenario | Solver to use |
|----------|--------------|
| Static stress analysis — linear | CalculiX `static` or Mystran or Z88 |
| Static stress analysis — nonlinear (large deflection, plasticity) | CalculiX `static` + Geometrical Nonlinearity |
| Modal (natural frequency) analysis | CalculiX `frequency` |
| Buckling analysis | CalculiX `buckling` |
| Steady-state or transient heat conduction | CalculiX `thermomech` or Elmer Heat |
| Thermal stress (heat + structural) | CalculiX `thermomech` (coupled) |
| Fluid flow (CFD) | Elmer Flow |
| Conjugate heat transfer (fluid + solid) | Elmer Flow + Heat |
| Electrostatic field | CalculiX `electromagnetic` or Elmer Electrostatic |
| DC current flow and Joule heating | Elmer Static Current + Heat |
| AC electromagnetic / eddy currents | Elmer Magnetodynamic |
| MEMS / electromechanical coupling | Elmer Electrostatic + Electricforce + Elasticity |

---

## Common setup errors

| Error | Likely cause | Fix |
|-------|-------------|-----|
| Solver runs but produces zero displacement | No load applied, or loads applied to wrong face | Verify loads are on the loaded face; check magnitudes are non-zero |
| Solver fails: "No boundary conditions" | Missing required BC for the chosen equation/analysis type | Add at minimum one Dirichlet BC (fixed constraint, temperature) |
| Solver fails: "No material" | Analysis has no material assigned to the mesh | Add a Material Solid and assign it to the body |
| CalculiX frequency: zero modes | All DOF are fixed (fully clamped) | Remove excess fixity — leave enough free DOF for vibration |
| Elmer won't converge (Flow) | Turbulent flow with laminar Navier-Stokes | Reduce Reynolds number or use simpler geometry |
| CalculiX static: large displacement, wrong results | Nonlinear problem solved linearly | Enable GeometricalNonlinearity = nonlinear |

---

## Python API

```python
import ObjectsFem
import FreeCAD as App

doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

# Add a CalculiX solver
calc = ObjectsFem.makeSolverCalculiX(doc, "CalculiX")
calc.AnalysisType = "static"
calc.GeometricalNonlinearity = "linear"
analysis.addObject(calc)

# Add an Elmer solver
elmer = ObjectsFem.makeSolverElmer(doc, "Elmer")
analysis.addObject(elmer)

# Add Elmer equations
heat_eq = ObjectsFem.makeEquationHeat(doc, elmer)
flow_eq = ObjectsFem.makeEquationFlow(doc, elmer)

doc.recompute()
```

---

## See also

- [Solver CalculiX](../tools/solver-calculix.md) — CalculiX configuration and properties
- [Solver Elmer](../tools/solver-elmer.md) — Elmer configuration and properties
- [Solver Mystran](../tools/solver-mystran.md) — Mystran configuration
- [Solver Z88](../tools/solver-z88.md) — Z88 configuration
- [Run Solver](../tools/run-solver.md) — executing the solver
- [FEM Workbench](../index.md) — workbench overview
