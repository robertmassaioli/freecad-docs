# Rack and Pinion Joint

> **In one sentence:** Link a rotating pinion to a sliding rack so that rotation of one automatically drives translation of the other — and vice versa.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Rack and Pinion  
**Shortcut:** `Q`

A Rack and Pinion joint couples a **Revolute joint** (the pinion) to a **Slider joint** (the rack) by a pitch-radius ratio. Rotating the pinion by one full revolution (2π radians) translates the rack by 2π × pitch radius. This is a *coupled* joint — it adds a kinematic ratio between two DOF, not a new geometric constraint.

---

## Intuition

A coupled joint does not introduce new geometric constraints between the parts. The parts must already be independently constrained by their own joints; the coupled joint just says: "when the pinion rotates by X, the rack must move by Y × X."

Think of a car's steering rack: the pinion (attached to the steering wheel shaft) rotates, and the rack (connecting to the front wheels) slides left or right. The ratio between the steering wheel rotation and the lateral rack travel is set by the pinion's pitch radius.

---

## When to use it

- Rack-and-pinion steering mechanisms.
- Linear actuators driven by a rotating motor (3-D printer Z-axis, CNC gantry).
- Any mechanism that converts rotary motion to linear motion via a toothed gear.

## When NOT to use it

- **Don't use this if the rotary-to-linear conversion is on the same axis** — that is a [Screw joint](joint-screw.md), not a Rack and Pinion.
- **Don't add this before the prerequisite joints exist** — the pinion must have a Revolute joint and the rack must have a Slider joint before this coupled joint can be added.

---

## Prerequisites

Before adding this joint:

1. The **pinion** must have a [Revolute joint](joint-revolute.md) connecting it to the frame.
2. The **rack** must have a [Slider joint](joint-slider.md) connecting it to the frame.

---

## Step-by-step

1. Press `Q` or choose **Assembly → Joints → Rack and Pinion**.
2. Click a geometry on the **pinion** (the rotating part) — typically the cylindrical face of the gear wheel.
3. Click a geometry on the **rack** (the sliding part) — typically a face defining the slide direction.
4. In the task panel, set the **Pitch Radius** (radius of the pinion pitch circle, in mm).
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Geometry on the pinion part |
| Reference 2 | Link | — | Geometry on the rack part |
| Pitch Radius | Length | — | Effective radius of the pinion pitch circle (mm) |

The rack travel per revolution = 2π × Pitch Radius.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "RackPinionCoupling")
joint.JointType = "RackPinion"
joint.Reference1 = (doc.getObject("Pinion"), ["Face2"])
joint.Reference2 = (doc.getObject("Rack"), ["Face1"])
joint.Radius = 15.0   # 15 mm pitch radius → 94.25 mm travel per revolution
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Coupled joint requires prerequisite kinematic joints first"
    Adding the Rack and Pinion joint before the pinion has a Revolute joint
    and the rack has a Slider joint will either fail or produce incorrect
    results. Always add kinematic joints first.

!!! warning "Pitch radius vs actual gear radius"
    The pitch radius is the radius of the pitch circle — typically smaller
    than the outer radius of the gear teeth. Using the outer radius gives the
    wrong travel ratio. Use the pitch circle radius (module × tooth count / 2).

---

## See also

- [Revolute Joint](joint-revolute.md) — the pinion's underlying joint
- [Slider Joint](joint-slider.md) — the rack's underlying joint
- [Screw Joint](joint-screw.md) — rotation → translation on the same axis
- [Simulation](simulation.md) — drive the pinion through a rotation range to animate
