# Materials

> **In one sentence:** Material objects assign physical properties — elastic
> modulus, density, thermal conductivity, and more — to bodies in the
> analysis, telling the solver how the material responds to applied loads.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Materials

| Tool | Description |
|------|-------------|
| [Material for Solid](#material-for-solid) | Assign solid material properties |
| [Material for Fluid](#material-for-fluid) | Assign fluid material properties |
| [Nonlinear Mechanical Material](#nonlinear-mechanical-material) | Add plasticity / nonlinear behaviour |
| [Reinforced Material (Concrete)](#reinforced-material-concrete) | Define reinforced concrete |
| [Material Editor](#material-editor) | Browse and edit the material database |

---

## Intuition

The solver works with numbers, not names. Assigning a material means giving
the solver the numerical values it needs:

- **Young's modulus (E)** — stiffness: how hard is it to stretch or compress?
- **Poisson's ratio (ν)** — how much does it contract sideways when pulled?
- **Density (ρ)** — mass per volume, needed for self-weight, inertia, and
  frequency analysis.
- **Thermal conductivity (λ)** — how fast does heat flow through it?
- **Specific heat capacity (c)** — how much energy is stored per degree?
- **Coefficient of thermal expansion (α)** — how much does it expand per °C?

A material object in FEM is a dictionary of these values. FreeCAD ships with
a library of standard materials (steel, aluminium, concrete, wood, etc.) that
you can select and then override individual properties.

**One material per body.** Each material object is assigned to one or more
bodies. If the model has multiple bodies with different materials (e.g. a
steel bolt in an aluminium bracket), create one material object per unique
material.

---

## Material for Solid

**Menu:** FEM → Model → Materials → Material for Solid

Creates a `Fem::Material` object for a solid (structural / thermal)
body. This is the most commonly used material tool.

### Step-by-step

1. Make sure the Analysis container is active.
2. Optionally select the body in the model tree (if not selected, the
   material applies to the whole analysis).
3. Choose **FEM → Model → Materials → Material for Solid**.
4. The Material task panel opens.

### Material task panel

The task panel has three sections:

**Body:** Select which body (or bodies) this material applies to. Leave
blank to apply to all bodies in the analysis.

**Material library:** A list of pre-defined materials. Click a name to
load its properties.

**Material properties table:** Edit individual properties. The key
properties for structural and thermal analysis:

| Property | Symbol | Unit | Typical steel value |
|----------|--------|------|---------------------|
| Young's Modulus | E | MPa | 210 000 MPa |
| Poisson's Ratio | ν | — | 0.30 |
| Density | ρ | kg/m³ | 7 900 kg/m³ |
| Yield Strength | σ_y | MPa | 250 MPa (A36 steel) |
| Thermal Conductivity | λ | W/(m·K) | 50 W/(m·K) |
| Specific Heat Capacity | c | J/(kg·K) | 500 J/(kg·K) |
| Thermal Expansion Coefficient | α | 1/K | 12×10⁻⁶ /K |

### Python API

```python
import FreeCAD as App
import ObjectsFem

doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

mat = ObjectsFem.makeMaterial(doc, "MaterialSteel")
mat.Category = "Solid"
mat.Material = {
    "Name": "Steel",
    "YoungsModulus": "210000 MPa",
    "PoissonRatio": "0.30",
    "Density": "7900 kg/m^3",
    "ThermalConductivity": "50 W/m/K",
    "SpecificHeat": "500 J/kg/K",
    "ThermalExpansionCoefficient": "12e-6 1/K",
}
analysis.addObject(mat)
doc.recompute()
```

---

## Material for Fluid

**Menu:** FEM → Model → Materials → Material for Fluid

Creates a material object for a fluid domain. Used for Elmer flow
simulations (Navier-Stokes). The key fluid properties are:

| Property | Symbol | Unit | Water at 20°C |
|----------|--------|------|---------------|
| Density | ρ | kg/m³ | 998 kg/m³ |
| Dynamic Viscosity | μ | Pa·s | 0.001 Pa·s |
| Kinematic Viscosity | ν | m²/s | 1.0×10⁻⁶ m²/s |
| Thermal Conductivity | λ | W/(m·K) | 0.598 W/(m·K) |
| Specific Heat | c | J/(kg·K) | 4182 J/(kg·K) |

The task panel and workflow are identical to Material for Solid, but the
property set is different.

---

## Nonlinear Mechanical Material

**Menu:** FEM → Model → Materials → Nonlinear Mechanical Material

Adds a **plasticity model** on top of an existing solid material. Elastic
(linear) analysis assumes stress is always proportional to strain. Nonlinear
analysis allows the material to yield — deform permanently when stress
exceeds the yield point.

### When to use it

- When stresses are expected to exceed the yield strength.
- When you need to predict permanent deformation (set) after unloading.
- Required for CalculiX nonlinear static analysis (`GeometricalNonlinearity`
  and material nonlinearity enabled).

### Plasticity models

| Model | Description |
|-------|-------------|
| Isotropic hardening | Yield surface expands uniformly; simple for most metals |
| Kinematic hardening | Yield surface translates; better for cyclic loading |

### Step-by-step

1. A **Material for Solid** must already exist in the analysis.
2. Select the solid material object in the model tree.
3. Choose **FEM → Model → Materials → Nonlinear Mechanical Material**.
4. A `FemMaterialMechanicalNonlinear` object appears under the solid material.
5. Edit its **Yield Strength** and **Hardening Slope** (slope of the
   stress-strain curve above yield).

### CalculiX solver setting

Nonlinear material requires the CalculiX solver's **Analysis Type** to be
`static` with **Geometrical Nonlinearity** set to `On`.

---

## Reinforced Material (Concrete)

**Menu:** FEM → Model → Materials → Reinforced Material (Concrete)

Defines a composite concrete-and-rebar material for reinforced concrete
structural analysis. The composite is modelled as two materials combined:

- **Matrix material** (concrete): compression-dominant, brittle.
- **Reinforcement material** (steel rebar): tension-carrying fibres.

The task panel lets you set the matrix material, the reinforcement material,
and the rebar angle (direction of the reinforcement fibres relative to
the global axes). Supported by the CalculiX solver for shell-based
reinforced-concrete structures.

---

## Material Editor

**Menu:** FEM → Model → Materials → Material Editor

Opens the FreeCAD material database editor — a browser for the built-in
material library and a form for editing all properties of any material.

### What you can do in the Material Editor

- Browse the library of built-in materials (metals, polymers, composites,
  fluids, electrical materials).
- View all properties of a selected material.
- Edit property values and save a custom material to disk (`.FCMat` format).
- Import/export `.FCMat` files for sharing materials between projects or teams.

### FCMat file format

FreeCAD materials are plain INI-style text files:

```ini
[General]
Name = My Steel
Author = R. Massaioli
Description = Custom steel alloy

[Mechanical]
YoungsModulus = 210000 MPa
PoissonRatio = 0.30
Density = 7900 kg/m^3
YieldStrength = 350 MPa
```

Custom materials should be saved to the user's material directory:
`~/.local/share/FreeCAD/Material/` (Linux) or the equivalent on other
platforms. They then appear in the material library automatically.

---

## Common mistakes and pitfalls

!!! warning "Units in material properties"
    All material property values must include their unit string in FreeCAD's
    quantity format (e.g. `"210000 MPa"`, not `"210000"`). A bare number
    without a unit string will cause the solver to use incorrect values.
    The material task panel handles units automatically; the issue arises
    only when setting properties via Python or editing FCMat files directly.

!!! warning "One material object per material type"
    Do not add two Material for Solid objects expecting them to apply to
    different sub-regions. Use the **Body** selector in the task panel to
    assign each material to the correct body. Creating two material objects
    without body assignments can cause both to apply to everything.

!!! warning "Yield Strength is for reference, not enforcement"
    The Yield Strength field in the linear elastic material is not used by
    the linear solver — it is stored for reference. Linear FEM does not
    model yielding. To simulate post-yield behaviour, use Nonlinear
    Mechanical Material and a nonlinear CalculiX analysis.

!!! warning "Density is required for self-weight and frequency"
    If you run a frequency (modal) or self-weight analysis and results seem
    wrong, check that the material has a non-zero Density. Frequency and
    inertial loads are mass-dependent; zero density produces infinite
    frequencies and zero self-weight.

---

## See also

- [Analysis Container](analysis.md) — the container that holds material objects
- Element Geometry — set cross-section properties for beam and shell elements
- Mechanical Constraints and Loads — apply forces and boundary conditions
- Solver CalculiX — run the analysis
