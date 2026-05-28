# Angle Joint

> **In one sentence:** Fix the angle between two axes or face normals to an arbitrary value — for setting rest positions, oblique mounts, and angular phase in linkages.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Angle  
**Shortcut:** `X`  
**DOF removed:** 1 | **DOF remaining:** 2 rotations + 3 translations

An Angle joint constrains one rotational DOF — the one that controls the angle between the two selected directions — to a specified value. All other rotations and all translations remain free.

---

## Intuition

The Angle joint is the general form of the [Perpendicular joint](joint-perpendicular.md) (90°) and of a zero-angle [Parallel joint](joint-parallel.md). Any specific angular relationship between two directions can be set with an Angle joint — 0°, 45°, 90°, or any value in between.

Use it to define the *rest position* or *phase* of a rotating part: a crank arm locked at 45° from horizontal, a latch cam locked at its open-position angle.

---

## When to use it

- Setting the rest angle of an angular output (e.g. crank arm at 45°, valve at its closed position).
- Fixing the angular phase between two links in a mechanism.
- Defining an oblique mounting angle for a component on a non-orthogonal surface.
- Locking the angle of a joint during assembly setup before the correct kinematic constraints are added.

## When NOT to use it

- **Don't use Angle for exactly 0°** — use [Parallel](joint-parallel.md) which is clearer in intent.
- **Don't use Angle for exactly 90°** — use [Perpendicular](joint-perpendicular.md).
- **Don't add Angle to a pair of parts that already has a Revolute on the same axis** — the Revolute removes 5 DOF including all off-axis rotation; the Angle joint would target the one remaining rotational DOF (the spin), but the resulting over-constraint depends on how many DOF the Angle actually removes. Check for solver errors.

---

## Step-by-step

1. Press `X` or choose **Assembly → Joints → Angle**.
2. Click a **cylindrical face, linear edge, or planar face** on Part 1.
3. Click the **corresponding geometry** on Part 2.
4. In the task panel, enter the **Angle** value in degrees.
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Direction-defining geometry on Part 1 |
| Reference 2 | Link | — | Direction-defining geometry on Part 2 |
| Angle | Angle | 0° | Angle between the two selected directions (degrees) |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "CrankAngle")
joint.JointType = "Angle"
joint.Reference1 = (doc.getObject("Frame"), ["Face1"])
joint.Reference2 = (doc.getObject("CrankArm"), ["Face3"])
joint.Angle = 45.0   # 45 degrees
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Reference direction ambiguity"
    The angle is measured between the two selected directions. A cylindrical
    face provides the cylinder axis; a planar face provides the face normal.
    Make sure the geometry you select gives the direction you intend — the
    angle is measured between those directions, not between the faces themselves.

!!! warning "Angle joint removes only 1 DOF"
    Two rotational and all three translational DOF remain free after applying
    an Angle joint. This is intentional for angular positioning, but if you
    expect the part to be more constrained, add further joints.

---

## See also

- [Parallel Joint](joint-parallel.md) — 0° angle (directions aligned)
- [Perpendicular Joint](joint-perpendicular.md) — 90° angle (special case)
- [Distance Joint](joint-distance.md) — maintain a fixed gap
- [Solve Assembly](solve.md) — apply joints
