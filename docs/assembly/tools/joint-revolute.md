# Revolute Joint

> **In one sentence:** Allow one part to spin freely around a single shared axis while preventing all other relative motion — the joint for shafts, hinges, and pins.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Revolute  
**Shortcut:** `R`  
**DOF removed:** 5 | **DOF remaining:** 1 (rotation about one axis)

A Revolute joint co-aligns two axes and keeps them co-linear while allowing free rotation about that shared axis. All translation and all off-axis rotation are locked.

---

## Intuition

Think of a door hinge. The hinge pin defines an axis; the door can only rotate about that axis — it cannot slide along the pin, cannot tilt sideways, cannot translate across the room. The Revolute joint encodes exactly this relationship.

The two selected cylindrical faces or circular edges define the axes that will be aligned. After solving, the axes are co-linear and the parts can rotate freely relative to each other about that axis.

---

## When to use it

- Shaft spinning inside a bearing or housing bore.
- Pin in a pin joint (clevis, hinge).
- Hinge between two panels or links.
- Gear shaft constrained by its housing (combine with a [Gears coupled joint](joint-gears.md) for the ratio).
- Any part that rotates about a known axis.

## When NOT to use it

- **Don't use Revolute if the shaft also slides axially** — use [Cylindrical](joint-cylindrical.md) instead, which allows both rotation and axial translation.
- **Don't use Revolute if only a fixed angle is needed** — use [Angle](joint-angle.md) to lock the angle without prescribing a physical axis connection.

---

## Step-by-step

1. Press `R` or choose **Assembly → Joints → Revolute**.
2. Click a **cylindrical face or circular edge** on **Part 1** — this defines the axis.
3. Click a **cylindrical face or circular edge** on **Part 2** — the mating axis.
4. Optionally set an **Offset** (axial distance between the two reference planes) and **Rotation** (initial angle) in the task panel.
5. Click **OK**.

The solver aligns the two axes co-linearly and allows only rotation between them.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Cylindrical face or circular edge on Part 1 |
| Reference 2 | Link | — | Cylindrical face or circular edge on Part 2 |
| Offset | Length | 0 mm | Axial gap between the two reference planes |
| Rotation | Angle | 0° | Initial rotation angle of Part 2 relative to Part 1 |

### Angular limits

After creation, the joint's **Data** properties include optional angular limits:

| Property | Description |
|----------|-------------|
| `EnableLimitAngle` | Toggle to enable min/max rotation limits |
| `LimitAngleMin` | Minimum allowed rotation (degrees) |
| `LimitAngleMax` | Maximum allowed rotation (degrees) |

Limits are enforced during simulation but do not prevent manual part movement in the 3-D view.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "ShaftBearing")
joint.JointType = "Revolute"
joint.Reference1 = (doc.getObject("Housing"), ["Face5"])
joint.Reference2 = (doc.getObject("Shaft"), ["Face2"])
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Selecting a flat face instead of a cylindrical face"
    A flat (planar) face does not define a unique axis — only a normal
    direction. The solver may produce unexpected results or refuse to solve.
    Always select a cylindrical face or a circular edge for Revolute joints.

!!! warning "Adding both Revolute and Fixed between the same parts"
    A Fixed joint already removes all 6 DOF including the rotational one.
    Adding a Revolute joint on top creates a conflict — 7 constraints on
    6 DOF. Delete the Fixed joint before adding Revolute.

!!! warning "Revolute vs Cylindrical for splined/sliding shafts"
    If the shaft must both spin AND slide axially (e.g. a splined shaft),
    use Cylindrical. Revolute locks axial translation.

---

## See also

- [Cylindrical Joint](joint-cylindrical.md) — rotation + axial translation on the same axis
- [Fixed Joint](joint-fixed.md) — lock all motion between two parts
- [Gears Joint](joint-gears.md) — couple two Revolute joints with a gear ratio
- [Belt Joint](joint-belt.md) — couple two Revolute joints with a belt/chain ratio
- [Solve Assembly](solve.md) — apply joints
