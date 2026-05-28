# Perpendicular Joint

> **In one sentence:** Force two axes or face normals to be exactly 90° apart — for right-angle drives, orthogonal frames, and brackets.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Perpendicular  
**Shortcut:** `M`  
**DOF removed:** 1 | **DOF remaining:** 2 rotations + 3 translations

A Perpendicular joint constrains one rotational DOF — the one that would change the angle between the two directions away from 90° — while leaving all other rotations and all translations free.

---

## Intuition

A Perpendicular joint is essentially an [Angle joint](joint-angle.md) locked at exactly 90°. Think of a right-angle bracket: no matter where or how the bracket is positioned, the two legs of the bracket must always be at 90° to each other. The Perpendicular joint enforces this angular relationship.

---

## When to use it

- Right-angle drive mechanisms (bevel gears, worm gears) where two shafts must be 90° apart.
- Structural frames and brackets where one member must be orthogonal to another.
- Linkage constraints where an output member must always stay perpendicular to its input.
- Assembly alignment when a part must be mounted at exactly 90° to a reference face.

## When NOT to use it

- **Don't use Perpendicular for an arbitrary angle** — use [Angle](joint-angle.md) instead.
- **Don't use Perpendicular in addition to a Revolute if they constrain the same rotation** — the Revolute already removes all off-axis rotations; adding Perpendicular for the same axes over-constrains the system.

---

## Step-by-step

1. Press `M` or choose **Assembly → Joints → Perpendicular**.
2. Click a **cylindrical face, linear edge, or planar face** on Part 1.
3. Click the **corresponding geometry** on Part 2.
4. Click **OK**.

The solver adjusts Part 2 so the two selected directions are exactly 90° apart.

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

joint = asm.Joints.newObject("Assembly::JointObject", "RightAngleDrive")
joint.JointType = "Perpendicular"
joint.Reference1 = (doc.getObject("InputShaft"), ["Face2"])
joint.Reference2 = (doc.getObject("OutputShaft"), ["Face2"])
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Reference geometry must unambiguously define a direction"
    Select a cylindrical face (defines the axis), a linear edge (defines the
    edge direction), or a planar face (defines the normal). Selecting a vertex
    does not define a direction and will fail or give unexpected results.

!!! warning "Perpendicular leaves 2 rotations free"
    This joint only removes 1 rotational DOF — the one that controls the
    angle between the two directions. Two rotational DOF remain free. If full
    orientation is needed, add further geometric joints.

---

## See also

- [Parallel Joint](joint-parallel.md) — force two directions to be parallel
- [Angle Joint](joint-angle.md) — fix an arbitrary angle (of which Perpendicular is a special case)
- [Distance Joint](joint-distance.md) — maintain a fixed gap
- [Solve Assembly](solve.md) — apply joints
