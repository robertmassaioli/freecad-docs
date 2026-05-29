# Centrifugal Load

> **In one sentence:** Apply centrifugal body loads to a rotating structure,
> computed from the rotational speed and distance from the rotation axis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Loads → Centrifugal Load

Applies centrifugal (centripetal) body loads to a rotating structure. The
load is computed from the rotational speed and the radial distance of each
element from the rotation axis.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Rotation Axis | A line or edge defining the rotation axis |
| Rotation Frequency | Angular velocity in rpm or Hz |

---

## Use cases

- Turbine blades, compressor discs, flywheels.
- Spinning shafts: hoop stress from rotation.
- Centrifuge rotors.

!!! warning "Density must be set"
    Centrifugal load is a body force proportional to mass. Without a non-zero
    density in the material, the load produces zero force.

---

## See also

- [Self Weight (Gravity)](load-self-weight.md) — gravitational body load
- [Material for Solid](material-solid.md) — density required for body loads
- [FEM Workbench](../index.md) — workbench overview
