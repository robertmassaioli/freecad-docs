# Parallel Joint

> **In one sentence:** Force two axes or face normals to point in the same direction — without fixing their position or spin — for belts, parallel linkages, and alignment constraints.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Parallel  
**Shortcut:** `N`  
**DOF removed:** 2 | **DOF remaining:** 1 spin + 3 translations

A Parallel joint aligns two selected directions (axes or face normals) so they point in the same direction. It removes 2 rotational DOF — the two tilts that would cause one direction to diverge from the other — while leaving the spin about the shared direction and all translations free.

---

## Intuition

Imagine two flagpoles that must always stay upright and parallel to each other, regardless of where they are placed. They can be at different heights, different lateral positions, and they can spin about their own vertical axes — but neither can tilt. A Parallel joint enforces this orientation relationship between two parts.

---

## When to use it

- Keeping two pulleys parallel when connected by a belt or chain (pair with a [Belt coupled joint](joint-belt.md) for the rotational link).
- Ensuring a moving plate or platform always stays level (parallel to the base).
- Constraining the output link of a four-bar linkage to remain parallel to the ground link (pantograph, Watt's parallel motion).
- Aligning a component that must track the orientation of another without being physically connected to it.

## When NOT to use it

- **Don't use Parallel if spin must also be locked** — Parallel leaves spin free. Add an [Angle](joint-angle.md) joint or [Distance](joint-distance.md) joint to fix the spin DOF.
- **Don't use Parallel if the parts are directly mated** — [Fixed](joint-fixed.md) or a kinematic joint (which enforces parallel as a by-product) is more appropriate.
- **Don't use Parallel in addition to a Revolute on the same axis** — the Revolute already constrains all off-axis rotation; adding Parallel would be redundant and over-constraining.

---

## Step-by-step

1. Press `N` or choose **Assembly → Joints → Parallel**.
2. Click a **cylindrical face, linear edge, or planar face** on Part 1 — defines the reference direction.
3. Click the **corresponding geometry** on Part 2.
4. Click **OK**.

The solver aligns the two directions. Translations and spin about the direction are not constrained.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Reference 1 | Link | Direction-defining geometry on Part 1 |
| Reference 2 | Link | Direction-defining geometry on Part 2 |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "ParallelShafts")
joint.JointType = "Parallel"
joint.Reference1 = (doc.getObject("DriveShaft"), ["Face2"])
joint.Reference2 = (doc.getObject("OutputShaft"), ["Face2"])
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Parallel does not prevent spin"
    A Parallel joint only aligns directions — the two parts can still spin
    about the shared direction independently. This is intentional: if you
    also need to lock spin, add an Angle joint (0°) or a Distance joint.

!!! warning "Adding Parallel to a part that already has a Revolute on the same axis"
    A Revolute joint between two parts already locks all rotations except
    the one about the shared axis. Adding a Parallel joint constraining that
    same axis removes a DOF that is already fully constrained, over-determining
    the system. Use one or the other.

---

## See also

- [Perpendicular Joint](joint-perpendicular.md) — force two directions to be 90° apart
- [Angle Joint](joint-angle.md) — fix an arbitrary angle between two directions
- [Distance Joint](joint-distance.md) — maintain a fixed gap
- [Belt Joint](joint-belt.md) — couple two parallel-axis revolutes with a belt ratio
- [Solve Assembly](solve.md) — apply joints
