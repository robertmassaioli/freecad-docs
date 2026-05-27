# Joints — Kinematic

> **In one sentence:** Kinematic joints define exactly how many and which
> degrees of freedom two parts share — from a fully rigid Fixed joint to a
> free-spinning Ball joint — and they form the building blocks of every
> moving mechanism in the Assembly workbench.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints

| Joint | Shortcut | DOF removed | DOF remaining | Description |
|-------|----------|-------------|---------------|-------------|
| [Fixed](#fixed-joint) | `F` | 6 | 0 | Lock two parts completely |
| [Revolute](#revolute-joint) | `R` | 5 | 1 (rotation) | Rotation around a single axis |
| [Cylindrical](#cylindrical-joint) | `C` | 4 | 2 (rot + trans) | Rotation and translation along one axis |
| [Slider](#slider-joint) | `S` | 5 | 1 (translation) | Linear motion along one axis only |
| [Ball](#ball-joint) | `B` | 3 | 3 (rotation) | Free rotation about a single point |

---

## Intuition

Every unconstrained part in 3-D space has **six degrees of freedom (DOF)**:
three translations (X, Y, Z) and three rotations (Rx, Ry, Rz). A joint
removes some of those DOF by declaring a geometric relationship between two
parts' coordinate systems.

The Assembly workbench solver counts DOF across the entire system:

```
Total DOF = (number of non-grounded parts × 6) − (sum of DOF removed by joints)
```

A fully-constrained, single-mechanism assembly should reach **0 residual DOF**
after grounding one part and adding joints. If the result is positive, the
assembly is under-constrained (some parts still float). If negative, it is
over-constrained (conflicting joints).

**Choosing the right joint type** is about matching the geometric intent:

- Shaft spinning in a bearing → Revolute (1 DOF: rotation about shaft axis)
- Lead screw → Screw coupled joint (see Joints — Coupled)
- Piston in a cylinder → Slider (1 DOF: translation along bore axis)
- Universal joint → Ball (3 DOF: free rotation about the ball centre)
- Glued or bolted parts → Fixed (0 DOF)

---

## Selecting geometry for joints

All kinematic joints require you to select geometry on each of the two parts.
The geometry tells the solver *where* and *in what direction* the joint acts:

- **Cylindrical face / circular edge** → defines an axis (revolute, cylindrical, slider)
- **Planar face** → defines a normal direction
- **Vertex / spherical face** → defines a point (ball joint)

The workflow is always:

1. Activate the joint tool (menu or shortcut).
2. Click a feature on **Part 1** (the reference part).
3. Click a feature on **Part 2** (the part to constrain).
4. Confirm in the task panel.

The solver snaps Part 2 so that its selected geometry aligns with Part 1's
selected geometry according to the joint's rules.

---

## Fixed joint

**Shortcut:** `F` | **DOF remaining:** 0

### What it does

A Fixed joint locks two parts together with no relative motion whatsoever.
All six DOF between the two parts are removed. The parts behave as if they
were welded or glued.

### When to use it

- Bolted flanges, glued surfaces, press-fit components.
- Sub-assemblies that are internally rigid — insert the sub-assembly and fix
  it to its mounting face.
- Temporarily fixing a part during assembly setup to simplify solving.

### Step-by-step

1. Press `F` or choose **Assembly → Joints → Fixed**.
2. Click a face, edge, or vertex on Part 1.
3. Click the corresponding feature on Part 2.
4. Click **OK** in the task panel.

The solver moves Part 2 so that its selected feature aligns with Part 1's
feature, then removes all relative motion.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Geometry on Part 1 (face, edge, vertex) |
| Reference 2 | Geometry on Part 2 |

### Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "Fixed")
joint.JointType = "Fixed"
joint.Reference1 = (doc.getObject("Plate"), ["Face1"])
joint.Reference2 = (doc.getObject("Bracket"), ["Face3"])
doc.recompute()
```

---

## Revolute joint

**Shortcut:** `R` | **DOF remaining:** 1 (rotation about one axis)

### What it does

A Revolute joint allows one part to rotate freely around a single axis while
all other relative motion is locked. The two selected axes are co-aligned and
kept co-linear; the only motion is spin about that shared axis.

### When to use it

- Shaft in a bearing or housing bore.
- Pin in a pin joint.
- Hinge between two panels.
- Any mechanism where one part spins relative to another about a known axis.

### Intuition

Think of a door hinge: the hinge axis is fixed in space; the door can only
rotate about that axis. All translation and all other rotations are prevented.

### Step-by-step

1. Press `R` or choose **Assembly → Joints → Revolute**.
2. Click a **cylindrical face or circular edge** on Part 1 (defines the axis).
3. Click a **cylindrical face or circular edge** on Part 2 (the mating axis).
4. Click **OK**.

The solver aligns the two axes and allows only rotation between them.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Cylindrical face or circular edge on Part 1 (axis provider) |
| Reference 2 | Cylindrical face or circular edge on Part 2 |
| Offset | Optional axial offset between the two reference planes |
| Rotation | Optional initial rotation angle |

### Limits

Revolute joints support optional **angular limits** (min/max rotation angle)
to prevent over-rotation. Set these in the joint's **Data** properties after
creation.

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "ShaftBearing")
joint.JointType = "Revolute"
joint.Reference1 = (doc.getObject("Housing"), ["Face5"])
joint.Reference2 = (doc.getObject("Shaft"), ["Face2"])
doc.recompute()
```

---

## Cylindrical joint

**Shortcut:** `C` | **DOF remaining:** 2 (rotation + translation along one axis)

### What it does

A Cylindrical joint allows both rotation *and* axial translation along a
single axis simultaneously. All other DOF are locked. It is the combination
of a Revolute and a Slider on the same axis.

### When to use it

- A shaft that can both spin and slide axially in a bearing (e.g. a splined
  shaft, a telescoping drive shaft).
- Any situation where coupling rotation and translation on one axis is
  required but they are *independent* (not linked by a lead-screw ratio —
  for that use Screw in Joints — Coupled).

### Intuition

A Cylindrical joint is what you get if you remove the axial lock from a
Revolute joint. The part can spin AND slide. If the rotation and translation
are linked by a ratio, use the Screw coupled joint instead.

### Step-by-step

1. Press `C` or choose **Assembly → Joints → Cylindrical**.
2. Click a cylindrical face or circular edge on Part 1.
3. Click the mating cylindrical face or edge on Part 2.
4. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Cylindrical face or circular edge on Part 1 |
| Reference 2 | Cylindrical face or circular edge on Part 2 |
| Offset | Optional offset along the axis |
| Rotation | Optional initial rotation |

---

## Slider joint

**Shortcut:** `S` | **DOF remaining:** 1 (translation along one axis)

### What it does

A Slider joint allows one part to translate linearly along a single axis
while all rotation and off-axis translation are locked. The selected faces
or edges define the slide direction.

### When to use it

- Piston in a cylinder bore.
- Dovetail slide or linear rail.
- Any part that moves in a straight line relative to another.

### Intuition

Think of a drawer: it slides in and out along one axis, but it cannot rotate
or move sideways. The Slider joint enforces exactly this.

### Step-by-step

1. Press `S` or choose **Assembly → Joints → Slider**.
2. Click a cylindrical face, linear edge, or planar face on Part 1 (defines
   the slide axis / direction).
3. Click the corresponding feature on Part 2.
4. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Axis-defining geometry on Part 1 |
| Reference 2 | Axis-defining geometry on Part 2 |
| Offset | Optional initial position along the axis |

### Limits

Slider joints support optional **distance limits** (min/max travel). Set in
the joint's **Data** properties after creation.

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "Piston")
joint.JointType = "Slider"
joint.Reference1 = (doc.getObject("Cylinder"), ["Face3"])
joint.Reference2 = (doc.getObject("Piston"), ["Face1"])
doc.recompute()
```

---

## Ball joint

**Shortcut:** `B` | **DOF remaining:** 3 (all rotations about one point)

### What it does

A Ball joint fixes the position of a point on Part 2 to a point on Part 1
(all three translations locked) while allowing completely free rotation about
that point (all three rotations free). It is the 3-D equivalent of a pivot
that can orient in any direction.

### When to use it

- Ball-and-socket joints (shoulder joint, trailer hitch, ball linkages).
- Spherical bearings.
- Rod ends on steering linkages or suspension arms.

### Intuition

The ball cannot leave the socket — translation is locked — but once inside
the socket, the ball can point in any direction. Three rotational DOF remain
free.

### Step-by-step

1. Press `B` or choose **Assembly → Joints → Ball**.
2. Click a **vertex, spherical face, or circular edge centre** on Part 1
   (the socket point).
3. Click the corresponding point on Part 2 (the ball centre).
4. Click **OK**.

The solver moves Part 2 so that its ball centre coincides with Part 1's
socket point. Part 2 can then rotate freely about that point.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Point or spherical face on Part 1 |
| Reference 2 | Point or spherical face on Part 2 |

### Note on under-constrainment

A Ball joint leaves 3 rotational DOF unconstrained. For mechanisms with ball
joints, these remaining rotational DOF must be constrained by other joints
or they will appear as warnings from the solver. For simulation purposes,
a single ball joint part (e.g. a pure link rod) usually needs a second joint
at the other end to fully constrain it.

---

## Common mistakes and pitfalls

!!! warning "Selecting the wrong geometry type"
    A Revolute joint needs a cylindrical axis — selecting a flat face gives
    the solver an ambiguous axis. A Ball joint needs a point — selecting a
    face gives it a plane. Use cylindrical faces or circular edges for axis
    joints; vertices or spherical faces for point joints.

!!! warning "Over-constraining with redundant joints"
    Adding both a Revolute and a Fixed joint between the same pair of parts
    over-constrains the system (the Fixed joint already removes all DOF
    including rotation). The solver will report a conflict. Use the minimum
    set of joints needed.

!!! warning "Under-constraining: leaving DOF unaccounted"
    A Ball joint between two parts still leaves 3 rotational DOF. If the
    remaining DOF are not constrained by other joints, Solve Assembly will
    report the assembly as under-constrained. This is correct — decide whether
    those DOF need constraining or are intentionally free (a genuinely
    under-constrained mechanism).

!!! warning "Mixing joint types that share an axis"
    A Slider and a Revolute on the same axis between the same pair of parts
    is equivalent to a Cylindrical joint. Using all three at once would be
    over-constrained. Choose the correct single joint type.

---

## See also

- [Assembly Setup](create-assembly.md) — insert parts and ground one before adding joints
- [Toggle Grounded](grounded.md) — fix the reference part
- [Joints — Geometric](joints-geometric.md) — distance, parallel, perpendicular, angle constraints
- [Joints — Coupled Motion](joints-coupled.md) — rack-and-pinion, screw, gears, belt
- [Solve Assembly](solve.md) — run the solver after adding joints
