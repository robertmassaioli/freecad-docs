# Gears Joint

> **In one sentence:** Couple two revolving parts with a gear ratio so that when one rotates, the other automatically rotates in the opposite direction at the defined speed ratio.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Gears  
**Shortcut:** `T`

A Gears joint couples two **Revolute joints** so that one gear's rotation drives the other in the **opposite direction** at a ratio determined by their pitch radii. This models external spur gear pairs, bevel gears, and any external gear mesh where the teeth interlock and reverse direction.

**Gear ratio:** ω₂ / ω₁ = −R₁ / R₂ (negative because external gears reverse rotation direction)

---

## Intuition

A coupled joint does not enforce physical contact between the gears — it just says "when Gear 1 rotates by X, Gear 2 must rotate by −(R₁/R₂) × X." The gears must be positioned correctly by their own Revolute joints; the Gears coupled joint purely governs the angular relationship between them during simulation.

Think of a simple spur gear pair: push one gear clockwise, the other turns counter-clockwise. The speed ratio is inversely proportional to the tooth counts, which corresponds to the ratio of pitch radii.

---

## When to use it

- External spur gear pairs (teeth on the outside of both gears, direction reverses).
- Bevel gear pairs (shafts at an angle, direction reverses at the mesh point).
- Any gearbox reduction or step-up stage with external meshing teeth.
- Modelling a gear train where the output speed and direction must be correct for simulation.

## When NOT to use it

- **Don't use Gears if the drive is a belt or chain** — use [Belt](joint-belt.md) instead, which preserves rotation direction.
- **Don't use Gears for internal gear meshes (ring gear + planet)** — use [Belt](joint-belt.md) because internal meshes preserve direction.
- **Don't add Gears before both Revolute joints exist** — each gear must already have its own Revolute joint.

---

## Prerequisites

Both gears must each have their own [Revolute joint](joint-revolute.md) connecting them to the housing or frame.

---

## Step-by-step

1. Press `T` or choose **Assembly → Joints → Gears**.
2. Click a **cylindrical face** on **Gear 1** — defines the axis and pitch radius 1.
3. Click a **cylindrical face** on **Gear 2** — defines the axis and pitch radius 2.
4. Set **Radius 1** and **Radius 2** (pitch circle radii in mm).
5. Click **OK**.

The gear ratio is automatically computed as −Radius 1 / Radius 2.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Cylindrical face on Gear 1 |
| Reference 2 | Link | — | Cylindrical face on Gear 2 |
| Radius 1 | Length | — | Pitch circle radius of Gear 1 (mm) |
| Radius 2 | Length | — | Pitch circle radius of Gear 2 (mm) |

**Computing pitch radius from gear specifications:**

```
pitch_radius = (module × tooth_count) / 2
```

Example: module 2, 20 teeth → pitch radius = (2 × 20) / 2 = 20 mm.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "GearMesh")
joint.JointType = "Gears"
joint.Reference1 = (doc.getObject("PinionGear"), ["Face2"])
joint.Reference2 = (doc.getObject("RingGear"), ["Face2"])
joint.Radius1 = 20.0   # 20 mm pinion pitch radius
joint.Radius2 = 60.0   # 60 mm gear pitch radius → 3:1 reduction
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Gears vs Belt: direction reversal"
    External gear meshes reverse rotation direction — use Gears. Belt and
    chain drives preserve rotation direction — use Belt. Choosing the wrong
    one produces a mechanism that rotates the opposite way in simulation.

!!! warning "Gears joint does not physically constrain gear mesh position"
    The Gears joint only adds the angular ratio. It does not ensure the gears
    are positioned with their pitch circles tangent. The gears must be correctly
    positioned by their Revolute joints and, if needed, a Distance joint between
    their axes.

!!! warning "Pitch radius vs outer radius"
    Enter the pitch circle radius, not the tip (outer) radius. The pitch circle
    is inside the tooth tips. Using the outer radius gives the wrong ratio.

---

## See also

- [Belt Joint](joint-belt.md) — same-direction rotation (belt, chain, internal gear)
- [Revolute Joint](joint-revolute.md) — the underlying joint for each gear
- [Screw Joint](joint-screw.md) — rotation-to-translation coupling
- [Simulation](simulation.md) — animate the gear mesh
