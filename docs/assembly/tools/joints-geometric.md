# Joints — Geometric

> **In one sentence:** Geometric joints constrain orientation and distance
> between parts without prescribing a single motion axis — making two faces
> parallel, two axes perpendicular, or maintaining a set gap between surfaces.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints

| Joint | Shortcut | Description |
|-------|----------|-------------|
| [Distance](#distance-joint) | `D` | Maintain a fixed distance or separation |
| [Parallel](#parallel-joint) | `N` | Make two axes or faces parallel |
| [Perpendicular](#perpendicular-joint) | `M` | Make two axes perpendicular |
| [Angle](#angle-joint) | `X` | Fix the angle between two axes |

---

## Intuition

Kinematic joints (Revolute, Slider, etc.) are specialised: they pick a
geometric arrangement that directly corresponds to a physical connection like
a shaft in a bearing or a piston in a bore. Geometric joints are more general
— they express relationships like:

- "These two faces must stay 5 mm apart."
- "This shaft must run parallel to that rail."
- "The output arm must be at 45° to the input arm."

These constraints appear throughout real mechanisms:

- A **Distance** joint models a gap, a clearance fit, or a specific stand-off
  height without fixing an axis.
- A **Parallel** joint locks orientation without fixing position — useful when
  two parts must track each other's direction.
- A **Perpendicular** joint enforces a 90° relationship commonly seen in
  right-angle drives, brackets, and orthogonal linkages.
- An **Angle** joint sets an arbitrary angle between two directions —
  essential for angular positioning of output members in a linkage.

Geometric joints are frequently combined with kinematic joints: a part might
be constrained by a Revolute joint (it can only rotate around an axis) and
an Angle joint (its rotation is locked to a specific angle).

---

## Distance joint

**Shortcut:** `D`

### What it does

The Distance joint maintains a fixed scalar distance between two selected
geometric elements. The elements can be:

- Two **planar faces** → perpendicular distance between the planes
- Two **vertices or spherical surfaces** → centre-to-centre distance
- A **vertex and a plane** → normal distance from the point to the plane

The distance can be zero (coincident / touching), positive (gap), or negative
(overlap, if geometrically valid for the selection type).

### When to use it

- Controlling a gap or clearance between two surfaces.
- Setting a specific stand-off between a PCB and an enclosure wall.
- Constraining a part to float at a known height above a base plate while
  still being free to translate laterally.
- Implementing a joint that maintains a minimum separation (combined with
  simulation limits).

### Step-by-step

1. Press `D` or choose **Assembly → Joints → Distance**.
2. Click the first geometric element (face, edge, vertex).
3. Click the second geometric element.
4. In the task panel, set the **Distance** value.
5. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Face, edge, or vertex on Part 1 |
| Reference 2 | Face, edge, or vertex on Part 2 |
| Distance | Scalar distance to maintain (mm) |

### Python API

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

## Parallel joint

**Shortcut:** `N`

### What it does

The Parallel joint forces two selected axes or planar normals to be parallel.
It constrains 2 rotational DOF (the two rotations that would tilt one axis
away from the other), leaving the third rotation (spin about the shared
direction) free and all translations free.

### When to use it

- Keeping two shafts parallel when they are connected by a belt or chain
  (combined with a Belt coupled joint for the rotational link).
- Ensuring a moving plate always stays level (parallel to the base plate).
- Constraining the output link of a four-bar linkage to remain parallel to
  the ground link (Watt's parallel motion, pantograph mechanisms).

### Step-by-step

1. Press `N` or choose **Assembly → Joints → Parallel**.
2. Click a **cylindrical face, linear edge, or planar face** on Part 1
   (defines the reference direction).
3. Click the corresponding geometry on Part 2.
4. Click **OK**.

The solver aligns the two directions. Translations and spin are not
constrained.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Direction-defining geometry on Part 1 |
| Reference 2 | Direction-defining geometry on Part 2 |

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "ParallelShafts")
joint.JointType = "Parallel"
joint.Reference1 = (doc.getObject("DriveShaft"), ["Face2"])
joint.Reference2 = (doc.getObject("OutputShaft"), ["Face2"])
doc.recompute()
```

---

## Perpendicular joint

**Shortcut:** `M`

### What it does

The Perpendicular joint forces two selected axes or planar normals to be
exactly 90° apart. It constrains 1 rotational DOF (the one that would change
the angle away from 90°), leaving the other rotations and all translations
free.

### When to use it

- Right-angle drive mechanisms (bevel gears, worm gears) where two shafts
  must be at 90°.
- Brackets and frames where one member must be perpendicular to another.
- Constraint in a linkage where an output must stay orthogonal to an input
  regardless of position.

### Step-by-step

1. Press `M` or choose **Assembly → Joints → Perpendicular**.
2. Click a direction-defining geometry on Part 1.
3. Click the corresponding geometry on Part 2.
4. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Direction-defining geometry on Part 1 |
| Reference 2 | Direction-defining geometry on Part 2 |

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "RightAngleDrive")
joint.JointType = "Perpendicular"
joint.Reference1 = (doc.getObject("InputShaft"), ["Face2"])
joint.Reference2 = (doc.getObject("OutputShaft"), ["Face2"])
doc.recompute()
```

---

## Angle joint

**Shortcut:** `X`

### What it does

The Angle joint fixes the angle between two selected axes or planar normals
to an arbitrary value (not necessarily 0° or 90°). It constrains 1 rotational
DOF while leaving other rotations and all translations free.

### When to use it

- Setting the rest position of an angular output (e.g. crank arm at 45° from
  horizontal).
- Fixing the angle between two links in a mechanism at a specific phase.
- Defining an oblique mounting angle for a component.

### Step-by-step

1. Press `X` or choose **Assembly → Joints → Angle**.
2. Click a direction-defining geometry on Part 1.
3. Click the corresponding geometry on Part 2.
4. In the task panel, enter the **Angle** value in degrees.
5. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Direction-defining geometry on Part 1 |
| Reference 2 | Direction-defining geometry on Part 2 |
| Angle | Angle between the two directions (degrees) |

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "CrankAngle")
joint.JointType = "Angle"
joint.Reference1 = (doc.getObject("Frame"), ["Face1"])
joint.Reference2 = (doc.getObject("CrankArm"), ["Face3"])
joint.Angle = 45.0   # 45 degrees
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Distance joint sign and direction"
    The sign of the Distance value depends on the orientation of the face
    normals. If the solver positions the part on the wrong side of the
    reference, try negating the distance value or selecting the opposite face.

!!! warning "Parallel and Perpendicular leave spin unconstrained"
    A Parallel joint only aligns directions — it does not prevent the part
    from spinning about that direction. If spin must also be constrained, add
    a second geometric joint (e.g. a Distance or Angle) or use a kinematic
    joint instead.

!!! warning "Angle joint reference direction ambiguity"
    The angle is measured between the two selected directions. If you select
    a cylindrical face, the solver picks the cylinder axis. If you select a
    planar face, it picks the face normal. Make sure the selected feature
    gives the direction you intend.

!!! warning "Over-constraining with geometry plus kinematics"
    A Revolute joint already removes 5 DOF including all off-axis rotation.
    Adding a Parallel joint between the same two parts attempts to remove a
    DOF that is already removed — the system becomes over-constrained. Use
    geometric joints only for DOF not already covered by kinematic joints.

---

## See also

- [Joints — Kinematic](joints-kinematic.md) — Fixed, Revolute, Cylindrical, Slider, Ball
- Joints — Coupled Motion — rack-and-pinion, screw, gears, belt
- Solve Assembly — run the solver after adding joints
- [Toggle Grounded](grounded.md) — fix the reference part first
