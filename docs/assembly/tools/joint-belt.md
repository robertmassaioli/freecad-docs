# Belt Joint

> **In one sentence:** Couple two revolving parts so that when one rotates, the other rotates in the same direction at the defined speed ratio — the joint for belt drives, chain drives, and internal gear meshes.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Belt  
**Shortcut:** `L`

A Belt joint couples two **Revolute joints** so that rotation of one pulley drives the other in the **same direction** at a ratio determined by their effective radii. This models belt drives, chain drives, and internal gear meshes — all cases where the coupling preserves rotation direction.

**Speed ratio:** ω₂ / ω₁ = +R₁ / R₂ (positive because direction is preserved)

---

## Intuition

A coupled joint does not model the physical belt or chain — it only enforces the angular ratio. The pulleys must be positioned correctly by their own Revolute joints; the Belt joint says "when Pulley 1 rotates by X, Pulley 2 must rotate by +(R₁/R₂) × X."

Think of a bicycle: the pedal sprocket turns, the rear wheel sprocket turns in the same direction via the chain. The speed ratio is set by the sprocket sizes (tooth counts, which correspond to pitch radii). A timing belt drive in a 3-D printer works the same way.

---

## When to use it

- Timing belt drives (3-D printer axes, CNC machines).
- V-belt or flat-belt drives.
- Roller chain drives (bicycle, conveyor, motorcycle).
- Internal gear meshes (ring gear + planet gear, where direction is preserved at the mesh).

## When NOT to use it

- **Don't use Belt for external spur gear meshes** — external gears reverse direction, use [Gears](joint-gears.md).
- **Don't add Belt before both Revolute joints exist** — each pulley must already have its own Revolute joint.

---

## Prerequisites

Both pulleys must each have their own [Revolute joint](joint-revolute.md) connecting them to the frame.

---

## Step-by-step

1. Press `L` or choose **Assembly → Joints → Belt**.
2. Click a **cylindrical face** on **Pulley 1**.
3. Click a **cylindrical face** on **Pulley 2**.
4. Set **Radius 1** and **Radius 2** (effective pulley radii in mm).
5. Click **OK**.

The speed ratio is automatically computed as Radius 1 / Radius 2.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Cylindrical face on Pulley 1 |
| Reference 2 | Link | — | Cylindrical face on Pulley 2 |
| Radius 1 | Length | — | Effective radius of Pulley 1 (mm) |
| Radius 2 | Length | — | Effective radius of Pulley 2 (mm) |

**For toothed belt / chain drives**, the effective radius is the pitch radius:

```
pitch_radius = (pitch × tooth_count) / (2π)
```

**For friction belt drives** (V-belt, flat belt), use the pulley groove radius.

---

## Choosing Gears vs Belt

| Situation | Use |
|-----------|-----|
| External gear mesh — teeth interlock, direction reverses | [Gears](joint-gears.md) |
| Belt / chain drive — direction preserves | Belt |
| Internal gear mesh — direction preserves | Belt |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "TimingBelt")
joint.JointType = "Belt"
joint.Reference1 = (doc.getObject("MotorPulley"), ["Face2"])
joint.Reference2 = (doc.getObject("DrivenPulley"), ["Face2"])
joint.Radius1 = 15.0   # 15 mm motor pulley
joint.Radius2 = 45.0   # 45 mm driven pulley → 3:1 reduction, same direction
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Belt vs Gears direction confusion"
    External spur gears reverse direction → Gears. Belt and chain drives
    preserve direction → Belt. Using the wrong joint makes the mechanism
    rotate the wrong way in simulation.

!!! warning "Belt joint does not model the physical belt or chain"
    The Belt joint adds only the angular ratio. It does not model belt
    tension, slip, or physical contact. The pulleys must be correctly
    positioned by their Revolute joints independently.

!!! warning "Parallel axis assumption"
    The Belt joint is designed for pulleys with parallel axes (standard belt
    or chain drive layout). For crossed-belt or twisted-chain configurations,
    use [Gears](joint-gears.md) with appropriate sign conventions.

---

## See also

- [Gears Joint](joint-gears.md) — opposite-direction coupling (external gear mesh)
- [Revolute Joint](joint-revolute.md) — the underlying joint for each pulley
- [Parallel Joint](joint-parallel.md) — keep the two pulley axes parallel
- [Simulation](simulation.md) — animate the belt drive
