# Thermal Boundary Conditions and Loads

> **In one sentence:** Thermal boundary conditions prescribe known
> temperatures or heat fluxes at the model boundaries, and thermal loads
> inject heat into the volume — together they define what temperature
> distribution the solver computes.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Thermal Boundary Conditions and Loads

| Tool | Type | Description |
|------|------|-------------|
| [Initial Temperature](#initial-temperature) | IC | Starting temperature for transient analysis |
| [Heat Flux Load](#heat-flux-load) | Load | Surface heat flux (W/m²) on a face |
| [Temperature Constraint](#temperature-constraint) | BC | Fix the temperature on a face or edge |
| [Body Heat Source](#body-heat-source) | Load | Volumetric heat generation rate (W/m³) |

---

## Intuition

A thermal simulation solves the heat equation: given heat sources, thermal
conductivity, and boundary temperatures, where does heat flow and what is
the resulting temperature field?

The boundary conditions follow the same Dirichlet / Neumann pattern as
structural analysis:

- **Dirichlet** — the temperature is *known* on a boundary (e.g. a
  face cooled by a heat sink held at 20 °C). This is the **Temperature
  Constraint**.
- **Neumann** — the heat flux is *known* on a boundary (e.g. 500 W/m²
  of radiative flux on an outer face). This is the **Heat Flux Load**.

Every thermal analysis needs at least one temperature constraint to fix a
reference temperature (analogous to a fixed constraint in structural
analysis preventing rigid body motion). Without it, the temperature field
is undefined.

---

## Initial Temperature

**Menu:** FEM → Model → Thermal BCs → Initial Temperature

Sets the uniform starting temperature for a **transient** (time-dependent)
thermal analysis. At time = 0, the entire model is at this temperature;
the solver then evolves the temperature distribution over time.

### When to use it

- Transient heat transfer: a part initially at room temperature is suddenly
  exposed to a high-temperature environment.
- Cool-down or warm-up analysis.

For **steady-state** analysis, the initial temperature is irrelevant —
the solver finds the time-independent solution regardless of the starting
point. Skip Initial Temperature for steady-state.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Initial Temperature | Starting temperature (°C or K) |

### Python API

```python
import ObjectsFem

doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

init_temp = ObjectsFem.makeConstraintInitialTemperature(doc, "InitTemp")
init_temp.initialTemperature = 20.0   # °C (20°C room temperature start)
analysis.addObject(init_temp)
doc.recompute()
```

---

## Heat Flux Load

**Menu:** FEM → Model → Thermal Loads → Heat Flux Load

Applies a surface heat flux to selected faces. The heat flux is a flow
of thermal energy through the surface, specified in watts per square metre
(W/m²).

### Flux types

FreeCAD's Heat Flux tool supports several surface boundary condition types:

| Type | Description | Equation |
|------|-------------|----------|
| Surface heat flux | Direct heat flow into/out of face | q = q₀ (W/m²) |
| Convective | Heat exchange with a surrounding fluid | q = h(T_∞ − T_surface) |
| Radiation | Radiative heat exchange with a black body | q = ε σ (T_amb⁴ − T_surface⁴) |

### Convective heat flux parameters

| Parameter | Symbol | Description |
|-----------|--------|-------------|
| Film coefficient | h | Convective heat transfer coefficient (W/m²/K) |
| Ambient temperature | T_∞ | Temperature of the surrounding fluid (°C) |

Typical convective coefficients:

| Condition | h (W/m²/K) |
|-----------|------------|
| Natural convection in air | 5 – 25 |
| Forced convection (fan) in air | 25 – 250 |
| Flowing water | 500 – 10 000 |
| Boiling water | 2 500 – 35 000 |

### Python API

```python
import ObjectsFem

heatflux = ObjectsFem.makeConstraintHeatflux(doc, "HeatFlux")
heatflux.AmbientTemp = 20.0   # °C
heatflux.FilmCoef = 25.0      # W/m²/K (forced convection in air)
heatflux.References = [(doc.getObject("Body"), "Face3")]
analysis.addObject(heatflux)
doc.recompute()
```

---

## Temperature Constraint

**Menu:** FEM → Model → Thermal BCs → Temperature Constraint

Fixes the temperature on selected faces, edges, or vertices to a prescribed
value. This is the thermal equivalent of the Fixed (or Displacement)
constraint in structural analysis.

### When to use it

- Face cooled by a heat sink at a known temperature.
- Face in contact with a constant-temperature reservoir (isothermal wall).
- Symmetry plane in a temperature-symmetric problem (zero heat flux across
  the plane, modelled by not applying a temperature constraint, or applying
  a specific temperature at a symmetry boundary).

### Parameters

| Parameter | Description |
|-----------|-------------|
| Temperature | Prescribed temperature (°C or K) |
| References | Face(s), edge(s), or vertex/vertices |

### Step-by-step

1. Select the face to constrain.
2. Choose **FEM → Model → Thermal BCs → Temperature Constraint**.
3. Enter the temperature value.
4. Click **OK**.

### Python API

```python
import ObjectsFem

temp_bc = ObjectsFem.makeConstraintTemperature(doc, "TempBC")
temp_bc.Temperature = 20.0   # °C
temp_bc.References = [(doc.getObject("Body"), "Face1")]
analysis.addObject(temp_bc)
doc.recompute()
```

---

## Body Heat Source

**Menu:** FEM → Model → Thermal Loads → Body Heat Source

Applies a volumetric heat generation rate to the entire body or selected
volumes. Heat is generated uniformly inside the material — modelling
resistive heating, chemical reaction exotherms, nuclear decay, or
frictional heating.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Heat Source | Volumetric heat generation rate (W/m³ or W/kg) |
| Mode | Total power (W) or power density (W/m³) |

### Common values

| Application | Approximate q (W/m³) |
|-------------|----------------------|
| Resistive heating, PCB component | 10 000 – 1 000 000 |
| Nuclear fuel element | up to 10¹⁰ |
| Solar energy absorbed in glass | ~100 – 1 000 |

### Python API

```python
import ObjectsFem

heat_src = ObjectsFem.makeConstraintBodyHeatSource(doc, "HeatSource")
heat_src.BodyHeatSource = 1e6   # W/m³
analysis.addObject(heat_src)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Missing temperature constraint → singular system"
    A pure Neumann thermal problem (only heat fluxes, no temperature
    constraint) is physically valid only if the net heat flux is zero.
    In practice, always include at least one Temperature Constraint to
    anchor the temperature scale; otherwise the solver produces an
    undefined (or very large) temperature field.

!!! warning "Transient vs steady-state and Initial Temperature"
    Initial Temperature only affects transient analysis. In steady-state
    mode, the solver ignores it. Conversely, for transient analysis, an
    Initial Temperature that does not match the Temperature Constraint
    values creates a thermal shock at t = 0 — which may or may not
    be physically intended.

!!! warning "Convection coefficient units"
    The film coefficient h is entered in W/m²/K. A common mistake is
    entering W/mm²/K (a factor of 10⁶ error). FreeCAD's thermal analysis
    typically works in SI units (m, kg, s, K) internally; check the unit
    system in Preferences → FEM → General.

!!! warning "Radiation is nonlinear"
    Radiative heat flux (q ∝ T⁴) is highly nonlinear. Including radiation
    in the Heat Flux tool requires a nonlinear solver iteration. Linear
    thermal analysis cannot handle radiation correctly. Enable nonlinear
    solving in the CalculiX solver settings when using radiation BCs.

---

## See also

- [Materials](materials.md) — thermal conductivity and specific heat required
- Mechanical Constraints and Loads — structural BCs for thermo-mechanical analysis
- Solver CalculiX — steady-state vs transient thermal, thermo-mechanical coupling
- Solver Elmer — Heat Equation for conduction/convection via Elmer
