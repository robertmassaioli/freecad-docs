# Distance Joint

> **In one sentence:** Maintain a fixed scalar distance between two faces, points, or vertices — controlling a gap, clearance, or stand-off without specifying a motion axis.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Distance  
**Shortcut:** `D`

A Distance joint constrains the scalar separation between two selected geometric elements to a specified value. Unlike kinematic joints, it does not prescribe a single motion axis — it only enforces a distance.

---

## Intuition

Kinematic joints (Revolute, Slider, etc.) are specialised: they say "this part moves *like a hinge*" or "this part moves *like a piston*." Geometric joints like Distance are more general — they just say "these two things must be this far apart."

This is useful wherever a gap, clearance, or stand-off must be controlled without fully constraining the part's motion type:
- A PCB must be exactly 3 mm above the enclosure floor — but can float laterally.
- A seal face must touch its mating surface (distance = 0).
- A safety guard must be at least 10 mm clear of a moving blade.

---

## When to use it

- Controlling a gap or clearance between two surfaces.
- Setting a specific stand-off between two planar faces.
- Constraining a part to float at a known height above a base plate while remaining free to translate laterally.
- Modelling a minimum-separation constraint when combined with simulation limits.

## When NOT to use it

- **Don't use Distance if you need full alignment** — a Distance between two planar faces only fixes the perpendicular gap, not lateral position or rotation. Add a [Parallel](joint-parallel.md) joint for orientation.
- **Don't use Distance when a specific axis of motion is intended** — use a [Slider](joint-slider.md) for pure linear motion along a defined axis.

---

## Step-by-step

1. Press `D` or choose **Assembly → Joints → Distance**.
2. Click the **first geometric element** (planar face, vertex, or spherical face) on Part 1.
3. Click the **second geometric element** on Part 2.
4. In the task panel, enter the **Distance** value (mm). Use 0 for contact/coincident.
5. Click **OK**.

### Supported geometry combinations

| Part 1 | Part 2 | Distance measured |
|--------|--------|-------------------|
| Planar face | Planar face | Perpendicular gap between planes |
| Vertex | Vertex | Centre-to-centre distance |
| Vertex | Planar face | Normal distance from point to plane |
| Spherical face | Spherical face | Centre-to-centre distance |

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Face, vertex, or spherical face on Part 1 |
| Reference 2 | Link | — | Face, vertex, or spherical face on Part 2 |
| Distance | Length | 0 mm | Scalar distance to maintain |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "Gap")
joint.JointType = "Distance"
joint.Reference1 = (doc.getObject("Base"), ["Face1"])
joint.Reference2 = (doc.getObject("Cover"), ["Face2"])
joint.Distance = 5.0   # 5 mm gap
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Distance sign depends on face normal orientation"
    If the solver positions the part on the wrong side of the reference (the
    gap appears on the opposite face), try negating the Distance value or
    selecting the opposite face. The sign of the distance is relative to the
    face normal directions.

!!! warning "Distance alone does not fully constrain a part"
    A Distance between two planar faces fixes one DOF (the normal gap).
    The part remains free to translate in the plane and to rotate. Add
    additional joints to fully constrain the part.

---

## See also

- [Parallel Joint](joint-parallel.md) — align two directions without fixing position
- [Perpendicular Joint](joint-perpendicular.md) — enforce a 90° relationship
- [Angle Joint](joint-angle.md) — fix an arbitrary angle between two directions
- [Solve Assembly](solve.md) — apply joints
