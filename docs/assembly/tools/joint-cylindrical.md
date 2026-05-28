# Cylindrical Joint

> **In one sentence:** Allow a part to both rotate and slide along a single axis simultaneously — the joint for splined shafts and telescoping drives.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Cylindrical  
**Shortcut:** `C`  
**DOF removed:** 4 | **DOF remaining:** 2 (rotation + translation along one axis)

A Cylindrical joint aligns two axes and keeps them co-linear while allowing both free rotation *and* free axial translation about that shared axis. All off-axis translation and off-axis rotation are locked. It is the combination of a Revolute and a Slider on the same axis — but with the rotation and translation completely independent of each other.

---

## Intuition

Imagine a pen cap sliding onto a pen barrel: the cap can both rotate around the barrel's axis and slide along it — two independent DOF on one axis, with no motion permitted in any other direction. A Cylindrical joint models exactly this.

The key distinction from a [Screw joint](joint-screw.md) is independence: in a Cylindrical joint the part can rotate without advancing and advance without rotating. In a Screw joint, rotation and translation are rigidly linked by a pitch ratio.

---

## When to use it

- A splined shaft that can both spin and slide axially in a splined bore (e.g. telescoping drive shaft, sliding gear selector).
- A telescoping tube where both rotation and length change are independent.
- Any mechanism where rotation and axial translation on one axis must be independently free.

## When NOT to use it

- **Don't use Cylindrical if rotation and translation are linked by a ratio** — use [Screw](joint-screw.md) instead (a Cylindrical joint with a fixed pitch coupling).
- **Don't use Cylindrical if the shaft cannot slide** — use [Revolute](joint-revolute.md) to lock axial translation.
- **Don't use Cylindrical if the shaft cannot rotate** — use [Slider](joint-slider.md) to lock rotation.

---

## Step-by-step

1. Press `C` or choose **Assembly → Joints → Cylindrical**.
2. Click a **cylindrical face or circular edge** on Part 1.
3. Click the **mating cylindrical face or edge** on Part 2.
4. Optionally set an **Offset** (initial axial position) and **Rotation** (initial angle) in the task panel.
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Cylindrical face or circular edge on Part 1 |
| Reference 2 | Link | — | Cylindrical face or circular edge on Part 2 |
| Offset | Length | 0 mm | Initial axial offset along the axis |
| Rotation | Angle | 0° | Initial rotation angle |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "SplinedShaft")
joint.JointType = "Cylindrical"
joint.Reference1 = (doc.getObject("OuterTube"), ["Face2"])
joint.Reference2 = (doc.getObject("InnerShaft"), ["Face1"])
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Cylindrical vs Screw for lead-screw mechanisms"
    Both allow rotation and translation on the same axis, but a Cylindrical
    joint makes them independent — the part can rotate without advancing. A
    lead screw requires a fixed ratio between rotation and advance. Use the
    [Screw joint](joint-screw.md) for that.

!!! warning "Cylindrical removes only 4 DOF"
    If your mechanism count shows more residual DOF than expected, check
    whether a Cylindrical joint was intended to be a Revolute (5 DOF removed).
    The extra free axial translation may be the source.

---

## See also

- [Revolute Joint](joint-revolute.md) — rotation only (locks axial translation)
- [Slider Joint](joint-slider.md) — translation only (locks rotation)
- [Screw Joint](joint-screw.md) — rotation and translation on one axis, linked by a pitch ratio
- [Solve Assembly](solve.md) — apply joints
