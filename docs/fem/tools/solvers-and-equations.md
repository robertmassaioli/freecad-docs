# Solvers and Equations

> **In one sentence:** Solvers are the external programs that compute the
> FEM solution; equations tell Elmer which physical PDEs to solve; and the
> Solver Control and Run tools manage the execution.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve

| Category | Tool | Description |
|----------|------|-------------|
| **Solvers** | [Solver CalculiX](#solver-calculix) | Structural, thermal, buckling, frequency, EM |
| | [Solver Elmer](#solver-elmer) | Multiphysics: flow, EM, heat, deformation |
| | [Solver Mystran](#solver-mystran) | Linear static structural (lightweight) |
| | [Solver Z88](#solver-z88) | Linear static structural |
| **Control** | [Solver Job Control](#solver-job-control) | Settings panel and output monitor |
| | [Run Solver](#run-solver) | Execute the active solver |
| **Elmer Equations** | [Deformation](#deformation-equation) | Large-strain solid deformation |
| | [Elasticity](#elasticity-equation) | Linear stress / elasticity |
| | [Electrostatic](#electrostatic-equation) | Electric potential and field |
| | [Electricforce](#electricforce-equation) | Electric force on bodies |
| | [Flow](#flow-equation) | Incompressible Navier-Stokes |
| | [Flux](#flux-equation) | Heat or electric flux |
| | [Heat](#heat-equation) | Heat conduction and convection |
| | [Magnetodynamic](#magnetodynamic-equation) | Time-harmonic AC EM fields |
| | [Magnetodynamic 2D](#magnetodynamic-2d-equation) | 2-D magnetodynamic |
| | [Static Current](#static-current-equation) | DC conduction current |

---

## Intuition

### Solvers vs equations

**Solvers** are the programs that solve the system of equations (CalculiX,
Elmer, Mystran, Z88). Each solver handles different physics:

- **CalculiX** — the most widely used for structural and thermal FEA in
  FreeCAD. Supports static, nonlinear, buckling, modal (frequency), and
  thermal analysis. Does not require separate equation objects — the
  analysis type is set in the solver's properties.

- **Elmer** — an open-source multiphysics solver that separates physics
  into **equation objects**. You add the Elmer solver, then add one or more
  equation objects to define what it solves. This gives great flexibility
  for coupled physics (e.g. flow + heat = conjugate heat transfer).

**Equations** are Elmer-only. Each equation corresponds to a set of PDEs.
You add equations to the analysis container alongside the Elmer solver.

---

## Solver CalculiX

**Menu:** FEM → Solve → Solver CalculiX

Adds a CalculiX solver object to the analysis. CalculiX (`ccx`) must be
installed and its path set in **Edit → Preferences → FEM → CalculiX**.

### Analysis types

Set the **Analysis Type** property of the solver object to choose what
CalculiX computes:

| Analysis Type | What it solves |
|---------------|---------------|
| `static` | Linear or nonlinear static deformation under loads |
| `frequency` | Natural frequencies and mode shapes (modal analysis) |
| `thermomech` | Thermal or thermo-mechanical analysis |
| `buckling` | Linear buckling load factor |
| `check` | Mesh quality check (no physics solved) |
| `electromagnetic` | Electrostatic analysis |

### Key properties

| Property | Description |
|----------|-------------|
| Analysis Type | See table above |
| Geometrical Nonlinearity | Enable large-deformation / nonlinear solve |
| Thermomech Type | Coupled / Uncoupled / Pure heat transfer |
| Iterations Control Max | Maximum nonlinear iterations |
| Split In Writers | Write results per load step (memory-efficient) |
| Output Frequency | Write results every N steps (transient) |
| Eigenmode Number | Number of modes to compute (frequency/buckling) |

### Static nonlinear analysis

For contact, plasticity, or large deformation:
1. Set Analysis Type = `static`.
2. Set Geometrical Nonlinearity = `On`.
3. Add Nonlinear Material if plasticity is needed.
4. Set Iterations Control Max to 100–200 (default 100).

### Frequency (modal) analysis

1. Set Analysis Type = `frequency`.
2. Set Eigenmode Number = number of modes to find (e.g. 10).
3. All loads are ignored (only stiffness and mass matter).
4. Results include natural frequencies (Hz) and mode shapes.

### Python API

```python
import ObjectsFem

doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

solver = ObjectsFem.makeSolverCalculiX(doc, "CalculiX")
solver.AnalysisType = "static"
solver.GeometricalNonlinearity = "linear"
analysis.addObject(solver)
doc.recompute()
```

---

## Solver Elmer

**Menu:** FEM → Solve → Solver Elmer

Adds an Elmer solver object to the analysis. Elmer (`ElmerSolver`) must be
installed and configured in **Edit → Preferences → FEM → Elmer**.

Unlike CalculiX, Elmer does not have a built-in analysis type — you define
what physics to solve by adding **equation objects** to the analysis.

### Elmer workflow

```
1. Add Solver Elmer to analysis
2. Add equation(s) (e.g. Heat Equation + Flow Equation for CHT)
3. Add material(s) with appropriate properties
4. Add boundary conditions matching the equations
5. Run Solver
```

### Key properties

| Property | Description |
|----------|-------------|
| Simulation Type | Steady State / Transient / Scanning |
| Time Step Intervals | Number of time steps (transient) |
| Timestep Size | Size of each time step (s) |
| Output Frequency | Write results every N steps |

---

## Solver Mystran

**Menu:** FEM → Solve → Solver Mystran

Adds a Mystran solver for linear static structural analysis. Mystran
(`mystran`) is a lightweight, fast open-source structural solver derived
from NASTRAN. Use it for quick linear structural checks when CalculiX is
not installed.

Mystran supports:
- Linear static analysis
- Shell and beam elements
- NASTRAN-format bulk data input

---

## Solver Z88

**Menu:** FEM → Solve → Solver Z88

Adds a Z88 solver for linear static structural analysis. Z88 (`z88r`) is
another lightweight open-source structural solver. Use when a fast linear
static analysis is needed and CalculiX is unavailable.

---

## Solver Job Control

**Menu:** FEM → Solve → Solver Job Control

Opens the solver job control panel — a combined settings dialog and output
monitor. From here you can:

- View and edit the solver's working directory.
- Open the generated solver input file for manual inspection or editing.
- Monitor the solver output log in real time as it runs.
- Re-read results from a previously completed solver run without re-running.

### Inspecting CalculiX input files

The CalculiX input file (`.inp` format) is written to the solver's working
directory. Open it in a text editor to:
- Verify the boundary conditions, material properties, and step definitions.
- Add CalculiX keywords not exposed in the FreeCAD UI (e.g. DLOAD, NLGEOM,
  CREEP).
- Debug unexpected solver failures.

### Working directory

Default: a temporary directory under `/tmp` (Linux/Mac) or `%TEMP%`
(Windows). Change it in Solver Job Control to a persistent location if you
want to keep solver input/output files.

---

## Run Solver

**Menu:** FEM → Solve → Run Solver

Writes the solver input files, launches the external solver executable, and
reads the result files when it finishes. This is the single "go" button.

**Result objects** appear in the analysis tree after a successful run.
If the solver fails, check the output log in Solver Job Control.

---

## Elmer Equations

Elmer equations are added to the analysis container alongside the Elmer
solver. Each equation tells Elmer which PDE to solve in the domain.

---

### Deformation Equation

Solves the large-deformation (geometrically nonlinear) solid mechanics
problem. Uses a total Lagrangian formulation and supports large strains.
More accurate than the Elasticity equation for large-displacement problems.

**Required BCs:** Displacement Constraint, Force Load, or similar
structural BCs.

---

### Elasticity Equation

Solves the linear elasticity PDE (small strain, linear material). This
is the standard structural stress analysis equation for most engineering
parts where strains are small (< ~5%).

**Required BCs:** Displacement Constraint (at minimum), Force / Pressure
load.

---

### Electrostatic Equation

Solves the Laplace / Poisson equation for the electric potential:
∇·(ε∇V) = −ρ_f. Computes the electric potential field and derives the
electric field **E** = −∇V.

**Required BCs:** Electrostatic Potential on conductors.
**Required material:** Dielectric permittivity (relative permittivity ε_r).

**Use cases:** Capacitor fields, insulation analysis, electrostatic
actuators, electrode field mapping.

---

### Electricforce Equation

Computes the electrostatic body force on dielectric or conducting bodies
due to the electric field. Typically coupled with the Electrostatic and
Elasticity equations to compute electrostatically-induced deformation
(electro-mechanical coupling).

---

### Flow Equation

Solves the incompressible Navier-Stokes equations for steady-state or
transient viscous fluid flow:

```
ρ(∂v/∂t + v·∇v) = −∇p + μ∇²v + f
∇·v = 0
```

**Required BCs:** Flow Velocity Constraint on inlets and walls, optionally
pressure at outlets.
**Required material:** Density and Dynamic Viscosity.

**Use cases:** Internal duct flow, external aerodynamics (low Re), coolant
flow in heat exchangers.

---

### Flux Equation

Computes the flux (heat flux or electric flux) from the gradient of a
scalar field already solved by another equation. Used after solving the
Heat or Electrostatic equation to compute derived flux quantities.

---

### Heat Equation

Solves the heat conduction-convection equation:

```
ρc(∂T/∂t) = ∇·(λ∇T) + Q
```

**Required BCs:** Temperature Constraint (Dirichlet), Heat Flux Load
(Neumann), or convective flux.
**Required material:** Thermal conductivity (λ), specific heat (c), density (ρ).

**Use cases:** Steady-state temperature distribution, transient cool-down,
conjugate heat transfer (coupled with Flow equation).

---

### Magnetodynamic Equation

Solves the time-harmonic (AC, frequency-domain) Maxwell equations for the
magnetic vector potential **A**. Computes magnetic field, eddy currents,
and Joule heating in conductors.

**Required BCs:** Magnetization or Current Density on source regions.
**Required material:** Electrical conductivity, relative permeability.

**Use cases:** Induction heating, transformer core analysis, eddy current
braking, wireless power transfer.

---

### Magnetodynamic 2D Equation

The 2-D (planar or axisymmetric) version of the Magnetodynamic equation.
Faster to solve and sufficient for many motor and transformer cross-section
analyses where the geometry is constant in the out-of-plane direction.

---

### Static Current Equation

Solves for the DC electric current distribution in a conductor:

```
∇·(σ∇V) = 0
```

where σ is the electrical conductivity. Computes current density and
Joule heating (I²R losses).

**Required BCs:** Electrostatic Potential on terminals.
**Required material:** Electrical conductivity.

**Use cases:** Busbar current distribution, PCB trace heating, grounding
electrode design.

---

## Equation–Solver–BC compatibility matrix

| Physics | Solver | Equation | Key BCs |
|---------|--------|----------|---------|
| Linear static | CalculiX | (built-in, static type) | Fixed, Force, Pressure |
| Modal (frequencies) | CalculiX | (built-in, frequency type) | Fixed only |
| Nonlinear static | CalculiX | (built-in, static + NLGEOM) | Fixed, Contact, Force |
| Buckling | CalculiX | (built-in, buckling type) | Fixed, Force |
| Steady thermal | CalculiX | (built-in, thermomech/pure heat) | Temp. Constraint, Heat Flux |
| Transient thermal | CalculiX | (built-in, thermomech) | Temp. Constraint, Init. Temp |
| Linear elasticity | Elmer | Elasticity | Displacement, Force |
| Large deformation | Elmer | Deformation | Displacement, Force |
| Electrostatics | Elmer | Electrostatic | Electrostatic Potential |
| Incompressible flow | Elmer | Flow | Flow Velocity Constraint |
| Heat transfer | Elmer | Heat | Temperature BC, Heat Flux |
| Magnetodynamics | Elmer | Magnetodynamic | Magnetization, Current Density |
| DC conduction | Elmer | Static Current | Electrostatic Potential |

---

## Common mistakes and pitfalls

!!! warning "CalculiX or Elmer binary not found"
    The most common setup error. After installing CalculiX or Elmer,
    set the executable path in **Edit → Preferences → FEM → CalculiX**
    (or Elmer). On Linux, `which ccx` gives the path. On Windows, it is
    wherever you extracted the CalculiX package.

!!! warning "Adding equations without the Elmer solver"
    Elmer equations are meaningless without the Elmer solver object in the
    analysis. If you have CalculiX as the solver, adding Elmer equations
    does nothing. Add FEM → Solve → Solver Elmer first.

!!! warning "CalculiX frequency analysis ignores loads"
    Modal (frequency) analysis computes the natural vibration modes. It
    does not apply force or pressure loads — those are irrelevant to the
    mode shapes. If you need stress under load AND natural frequencies,
    run two separate analyses: one static, one frequency.

!!! warning "Elmer requires named Gmsh mesh groups"
    Elmer identifies boundary faces by the group names assigned in the
    Gmsh mesh. If the mesh was generated with Netgen (no groups), Elmer
    cannot correctly assign boundary conditions. Use Gmsh + FEM Mesh Group
    for all Elmer analyses.

---

## See also

- [Mesh](mesh.md) — the mesh must be generated before running the solver
- [Mechanical Constraints and Loads](constraints-mechanical.md) — BCs for CalculiX structural
- [Thermal Constraints and Loads](constraints-thermal.md) — BCs for thermal analysis
- [Electromagnetic, Fluid and Geometrical Constraints](constraints-other.md) — BCs for Elmer
- Results — view and post-process solver output
