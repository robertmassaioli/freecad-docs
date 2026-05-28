# Slider Joint

> **In one sentence:** Allow a part to move linearly along one axis while locking all rotation and all off-axis translation — the joint for pistons, drawers, and linear rails.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Slider  
**Shortcut:** `S`  
**DOF removed:** 5 | **DOF remaining:** 1 (translation along one axis)

A Slider joint defines a slide direction and allows one part to translate freely along it while all other motion is locked. The selected geometry defines the slide axis or direction.

---

## Intuition

Think of a drawer in a cabinet. It slides in and out along one axis — forward and backward. It cannot rotate, cannot move sideways, cannot tip. A Slider joint enforces exactly this constraint.

---

## When to use it

- Piston inside a cylinder bore.
- Dovetail slide or T-slot rail carriage.
- Any part that moves in a straight line relative to another with no rotation.
- Rack in a [Rack and Pinion coupled joint](joint-rack-pinion.md) — the rack must have a Slider joint first.

## When NOT to use it

- **Don't use Slider if the part also rotates** — use [Cylindrical](joint-cylindrical.md) (independent rotation + translation) or [Screw](joint-screw.md) (linked rotation + translation).
- **Don't use Slider for curved paths** — Assembly joints only support straight-line translation. For curved paths, use a scripted simulation or multi-joint workaround.

---

## Step-by-step

1. Press `S` or choose **Assembly → Joints → Slider**.
2. Click a **cylindrical face, linear edge, or planar face** on Part 1 — this defines the slide direction (a cylindrical face uses its axis; a linear edge uses its direction; a planar face uses its normal).
3. Click the corresponding feature on Part 2.
4. Optionally set an **Offset** (initial position along the axis) in the task panel.
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Axis-defining geometry on Part 1 |
| Reference 2 | Link | — | Axis-defining geometry on Part 2 |
| Offset | Length | 0 mm | Initial position of Part 2 along the slide axis |

### Distance limits

After creation, the joint's **Data** properties include optional travel limits:

| Property | Description |
|----------|-------------|
| `EnableLimitLength` | Toggle to enable min/max travel limits |
| `LimitLengthMin` | Minimum travel position (mm) |
| `LimitLengthMax` | Maximum travel position (mm) |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "Piston")
joint.JointType = "Slider"
joint.Reference1 = (doc.getObject("Cylinder"), ["Face3"])
joint.Reference2 = (doc.getObject("Piston"), ["Face1"])
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Selecting a planar face gives translation along the normal, not in the plane"
    When you select a planar face, the slide direction is the face's *normal* —
    perpendicular to the face. If you want the part to slide *along* the face,
    select a linear edge that runs in the desired direction instead.

!!! warning "Slider + Revolute on the same axis = Cylindrical"
    A Slider and a Revolute joint between the same two parts, both on the same
    axis, is equivalent to a single Cylindrical joint. Using all three would
    be over-constrained. Choose the single joint type that matches the intended
    motion.

!!! warning "Rack must have a Slider before adding Rack and Pinion"
    The [Rack and Pinion coupled joint](joint-rack-pinion.md) requires the
    rack to already have a Slider joint. The coupled joint adds the ratio link
    on top of the existing Slider — it does not create the slider constraint.

---

## See also

- [Revolute Joint](joint-revolute.md) — rotation only
- [Cylindrical Joint](joint-cylindrical.md) — rotation + translation (independent)
- [Screw Joint](joint-screw.md) — rotation + translation (linked by pitch)
- [Rack and Pinion Joint](joint-rack-pinion.md) — couple this Slider to a Revolute
- [Solve Assembly](solve.md) — apply joints
