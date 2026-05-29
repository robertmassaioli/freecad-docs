# Heat Flux Load

> **In one sentence:** Apply a surface heat flux — direct, convective, or
> radiative — to selected faces as a Neumann thermal boundary condition.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Thermal Loads → Heat Flux Load

Applies a surface heat flux to selected faces. Supports direct heat flux
(W/m²), convective heat exchange with a fluid, and radiative heat exchange.

---

## Flux types

| Type | Description | Equation |
|------|-------------|----------|
| Surface heat flux | Direct heat flow | q = q₀ (W/m²) |
| Convective | Heat exchange with surrounding fluid | q = h(T_∞ − T_surface) |
| Radiation | Radiative exchange with a black body | q = εσ(T_amb⁴ − T_surface⁴) |

---

## Convective heat flux parameters

| Parameter | Symbol | Description |
|-----------|--------|-------------|
| Film coefficient | h | Convective heat transfer coefficient (W/m²/K) |
| Ambient temperature | T_∞ | Temperature of the surrounding fluid (°C) |

### Typical convective coefficients

| Condition | h (W/m²/K) |
|-----------|------------|
| Natural convection in air | 5 – 25 |
| Forced convection (fan) in air | 25 – 250 |
| Flowing water | 500 – 10 000 |

---

## Common mistakes and pitfalls

!!! warning "Convection coefficient units"
    The film coefficient h must be in W/m²/K, not W/mm²/K — a factor of 10⁶ 
    difference. Check the unit system in Preferences → FEM → General.

!!! warning "Radiation is nonlinear"
    Radiative flux (∝ T⁴) is nonlinear and requires a nonlinear solver iteration.
    Enable nonlinear solving in the CalculiX settings when using radiation BCs.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

heatflux = ObjectsFem.makeConstraintHeatflux(doc, "HeatFlux")
heatflux.AmbientTemp = 20.0
heatflux.FilmCoef = 25.0
heatflux.References = [(doc.getObject("Body"), "Face3")]
analysis.addObject(heatflux)
doc.recompute()
```

---

## See also

- [Temperature Constraint](constraint-temperature.md) — fix temperature on a boundary
- [Body Heat Source](load-body-heat-source.md) — volumetric heat generation
- [FEM Workbench](../index.md) — workbench overview
