# Screw Joint

> **In one sentence:** Link the rotation and axial translation of a part on the same axis by a thread pitch so that one full revolution advances the nut by exactly one pitch — the joint for lead screws and threaded fasteners.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Screw  
**Shortcut:** `W`

A Screw joint couples a **Revolute** and a **Slider** on the **same axis** by a pitch ratio. One full rotation advances the translating part by exactly one pitch length. This is a coupled joint — it does not add new geometric constraints but locks the ratio between the two existing DOF.

---

## Intuition

A lead screw works by converting rotation into precise linear motion: turn the screw one revolution, the nut advances by one pitch. A [Cylindrical joint](joint-cylindrical.md) also allows both rotation and translation on the same axis — but independently. The Screw joint removes that independence by saying "every degree of rotation corresponds to exactly this many mm of advance."

The physical analogy: a bolt being threaded into a nut. The bolt cannot rotate without advancing, and cannot advance without rotating.

---

## When to use it

- Lead screws in CNC machines, 3-D printers, and precision linear stages.
- Worm drives (combine with a [Gears coupled joint](joint-gears.md) on the output shaft for the reduction ratio).
- Any mechanism where rotation and linear advance are rigidly linked by a thread pitch.

## When NOT to use it

- **Don't use Screw if rotation and translation are independent** — use [Cylindrical](joint-cylindrical.md).
- **Don't use Screw if the linear motion is on a different axis from the rotation** — use [Rack and Pinion](joint-rack-pinion.md).

---

## Prerequisites

The two parts must already have joints that establish their independent motion:

- The **rotating part** needs a [Revolute joint](joint-revolute.md).
- The **translating part** needs a [Slider joint](joint-slider.md) on the same axis.

---

## Step-by-step

1. Press `W` or choose **Assembly → Joints → Screw**.
2. Click a **cylindrical face** on the rotating part (screw axis).
3. Click the corresponding axis geometry on the translating part.
4. In the task panel, set the **Pitch** (mm advance per full revolution).
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Link | — | Axis geometry on the rotating part |
| Reference 2 | Link | — | Axis geometry on the translating part |
| Pitch | Length | — | Linear advance per full revolution (mm/rev) |

**Common metric pitch values:**

| Thread | Pitch (mm/rev) |
|--------|---------------|
| M2 | 0.4 |
| M3 | 0.5 |
| M4 | 0.7 |
| M5 | 0.8 |
| M8 | 1.25 |
| T8×2 (lead screw) | 2.0 |
| T8×8 (4-start lead screw) | 8.0 |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "LeadScrew")
joint.JointType = "Screw"
joint.Reference1 = (doc.getObject("Screw"), ["Face3"])
joint.Reference2 = (doc.getObject("Nut"), ["Face1"])
joint.Pitch = 2.0   # 2 mm advance per revolution
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Adding Screw before the Revolute and Slider exist"
    The Screw joint requires the rotating part to have a Revolute joint and
    the translating part to have a Slider joint. Add those first.

!!! warning "Confusing Screw and Cylindrical"
    Both allow rotation and translation on the same axis. Cylindrical makes
    them independent; Screw locks them by a ratio. If you set up a Cylindrical
    joint and notice the simulated motion doesn't advance the nut, replace it
    with a Screw joint and set the pitch.

!!! warning "Pitch is per revolution, not per radian"
    FreeCAD's Pitch parameter is in mm per full revolution (360°), not per
    radian. A common mistake is entering the pitch in mm/rad which gives a
    ratio 2π times too small.

---

## See also

- [Revolute Joint](joint-revolute.md) — the rotating part's underlying joint
- [Slider Joint](joint-slider.md) — the translating part's underlying joint
- [Cylindrical Joint](joint-cylindrical.md) — rotation + translation without linking them
- [Rack and Pinion Joint](joint-rack-pinion.md) — rotation → translation on different axes
- [Simulation](simulation.md) — animate the screw mechanism
