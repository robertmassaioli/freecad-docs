# Body Heat Source

> **In one sentence:** Apply a volumetric heat generation rate (W/m³) to
> the body — for resistive heating, chemical exotherms, or nuclear decay.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Thermal Loads → Body Heat Source

Applies a volumetric heat generation rate to the entire body or selected
volumes. Heat is generated uniformly inside the material, modelling
resistive heating, chemical reaction exotherms, nuclear decay, or
frictional heating.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Heat Source | Volumetric heat generation rate (W/m³ or W/kg) |
| Mode | Total power (W) or power density (W/m³) |

---

## Common values

| Application | Approximate q (W/m³) |
|-------------|----------------------|
| Resistive heating, PCB component | 10 000 – 1 000 000 |
| Nuclear fuel element | up to 10¹⁰ |
| Solar energy absorbed in glass | ~100 – 1 000 |

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

heat_src = ObjectsFem.makeConstraintBodyHeatSource(doc, "HeatSource")
heat_src.BodyHeatSource = 1e6   # W/m³
analysis.addObject(heat_src)
doc.recompute()
```

---

## See also

- [Heat Flux Load](load-heat-flux.md) — surface heat flux
- [Temperature Constraint](constraint-temperature.md) — fix temperature on boundary
- [FEM Workbench](../index.md) — workbench overview
