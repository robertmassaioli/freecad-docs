# Joints — Coupled Motion

> **In one sentence:** Coupled-motion joints link the movement of two
> independently-jointed parts by a ratio — so that when one part rotates or
> translates, the other automatically follows at the defined speed or pitch.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints

| Joint | Shortcut | Description |
|-------|----------|-------------|
| [Rack and Pinion](#rack-and-pinion-joint) | `Q` | Link a slider to a revolute: rotation drives linear motion |
| [Screw](#screw-joint) | `W` | Link a slider to a revolute by a pitch ratio |
| [Gears](#gears-joint) | `T` | Link two revolutes with a gear ratio (opposite direction) |
| [Belt](#belt-joint) | `L` | Link two revolutes with a gear ratio (same direction) |

---

## Intuition

A kinematic joint (Revolute, Slider, etc.) defines *what kind of motion* a
part can make. A coupled joint defines *how that motion is shared* between
two parts that are each separately connected by their own kinematic joints.

The key insight is that coupled joints do **not** introduce new geometric
constraints — they add a **kinematic ratio** on top of existing DOF. The
two parts must already have their own joints; the coupled joint just says:
"when Part A moves by X, Part B must move by Y × X."

**Why this matters for simulation:** Without coupled joints, you can drive
one part and the others stay put. With coupled joints, driving the pinion
automatically moves the rack, or spinning one gear automatically spins the
meshing gear. This is what makes the Simulation tool useful for realistic
mechanism preview.

---

## Rack and Pinion joint

**Shortcut:** `Q`

### What it does

The Rack and Pinion joint couples a **Revolute joint** (the pinion) to a
**Slider joint** (the rack) so that rotating the pinion translates the rack
and vice versa. The coupling ratio is determined by the **Pitch Radius** of
the pinion: one full rotation (2π radians) moves the rack by 2π × radius.

### When to use it

- Rack-and-pinion steering mechanisms.
- Linear actuators driven by a rotating motor (e.g. 3-D printer Z-axis).
- Any mechanism that converts rotary motion to linear motion.

### Prerequisites

Before adding this joint:

1. The pinion must have a **Revolute joint** connecting it to the frame.
2. The rack must have a **Slider joint** connecting it to the frame.

### Step-by-step

1. Press `Q` or choose **Assembly → Joints → Rack and Pinion**.
2. Click a geometry on the **pinion** (the rotating part) — typically the
   cylindrical face of the gear wheel.
3. Click a geometry on the **rack** (the sliding part) — typically a face
   defining the slide direction.
4. In the task panel, set the **Pitch Radius** (radius of the pinion pitch
   circle, in mm).
5. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Geometry on the pinion part |
| Reference 2 | Geometry on the rack part |
| Pitch Radius | Effective radius of the pinion pitch circle (mm) |

### Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "RackPinionCoupling")
joint.JointType = "RackPinion"
joint.Reference1 = (doc.getObject("Pinion"), ["Face2"])
joint.Reference2 = (doc.getObject("Rack"), ["Face1"])
joint.Radius = 15.0   # 15 mm pitch radius
doc.recompute()
```

---

## Screw joint

**Shortcut:** `W`

### What it does

The Screw joint couples a **Revolute** and a **Slider** on the **same axis**
by a **pitch ratio**. One full rotation of the screw advances the nut (or
the screw itself) by exactly one pitch length. This is the physical model of
a lead screw, ball screw, or threaded fastener driving a nut.

The difference from a Cylindrical joint is that the Screw joint *locks* the
ratio between rotation and translation — a Cylindrical joint allows both DOF
independently, while a Screw joint removes one by coupling them.

### When to use it

- Lead screws in CNC machines, 3-D printers, and precision linear stages.
- Worm and worm gear (combined with a Gears joint on the output shaft).
- Any mechanism where rotation and linear advance are rigidly linked by a
  thread pitch.

### Prerequisites

The two parts must already have joints that establish their independent
motion:

- The rotating part needs a **Revolute joint**.
- The translating part needs a **Slider joint** on the same axis.

### Step-by-step

1. Press `W` or choose **Assembly → Joints → Screw**.
2. Click a cylindrical face on the rotating part (screw axis).
3. Click the corresponding axis geometry on the translating part.
4. Set the **Pitch** (advance per revolution, in mm).
5. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Axis geometry on the rotating part |
| Reference 2 | Axis geometry on the translating part |
| Pitch | Linear advance per full revolution (mm/rev) |

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "LeadScrew")
joint.JointType = "Screw"
joint.Reference1 = (doc.getObject("Screw"), ["Face3"])
joint.Reference2 = (doc.getObject("Nut"), ["Face1"])
joint.Pitch = 2.0   # 2 mm per revolution (M2 pitch)
doc.recompute()
```

---

## Gears joint

**Shortcut:** `T`

### What it does

The Gears joint couples two **Revolute joints** so that when one gear
rotates, the other rotates in the **opposite direction** at a ratio
determined by the ratio of their pitch radii. This models external spur
gears, bevel gears, and any gear mesh where the teeth interlock (external
contact reverses direction).

The gear ratio is: ω₂ / ω₁ = −R₁ / R₂ (negative because rotation reverses).

### When to use it

- Spur gear pairs, bevel gear pairs.
- Any external gear mesh (teeth on the outside of both gears).
- Modelling a gearbox reduction stage.

### Prerequisites

Both gears must each have their own **Revolute joint** connecting them to
the housing or frame.

### Step-by-step

1. Press `T` or choose **Assembly → Joints → Gears**.
2. Click a cylindrical face on **Gear 1** (defines axis and pitch radius 1).
3. Click a cylindrical face on **Gear 2** (defines axis and pitch radius 2).
4. Set **Radius 1** and **Radius 2** (pitch circle radii in mm).
5. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Cylindrical face on Gear 1 |
| Reference 2 | Cylindrical face on Gear 2 |
| Radius 1 | Pitch circle radius of Gear 1 (mm) |
| Radius 2 | Pitch circle radius of Gear 2 (mm) |

The gear ratio is automatically computed as Radius 1 / Radius 2.

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "GearMesh")
joint.JointType = "Gears"
joint.Reference1 = (doc.getObject("PinionGear"), ["Face2"])
joint.Reference2 = (doc.getObject("RingGear"), ["Face2"])
joint.Radius1 = 20.0   # pinion pitch radius
joint.Radius2 = 60.0   # ring gear pitch radius (3:1 reduction)
doc.recompute()
```

---

## Belt joint

**Shortcut:** `L`

### What it does

The Belt joint couples two **Revolute joints** so that when one pulley
rotates, the other rotates in the **same direction** at a ratio determined
by their radii. This models belt drives, chain drives, and internal gear
meshes — all cases where the coupling element (belt, chain) runs on the
outside of both wheels and preserves direction.

The ratio is: ω₂ / ω₁ = +R₁ / R₂ (positive because direction is preserved).

### When to use it

- Timing belt drives (3-D printers, CNC machines).
- V-belt or flat-belt drives in machinery.
- Chain drives (bicycle, conveyor).
- Internal gear meshes (ring gear, planetary stages).

### Prerequisites

Both pulleys must each have their own **Revolute joint**.

### Step-by-step

1. Press `L` or choose **Assembly → Joints → Belt**.
2. Click a cylindrical face on **Pulley 1**.
3. Click a cylindrical face on **Pulley 2**.
4. Set **Radius 1** and **Radius 2** (effective pulley radii in mm).
5. Click **OK**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reference 1 | Cylindrical face on Pulley 1 |
| Reference 2 | Cylindrical face on Pulley 2 |
| Radius 1 | Effective radius of Pulley 1 (mm) |
| Radius 2 | Effective radius of Pulley 2 (mm) |

### Gears vs Belt: choosing the right one

| Situation | Use |
|-----------|-----|
| External gear mesh (teeth interlock, direction reverses) | Gears |
| Internal gear mesh, belt drive, chain drive (direction preserves) | Belt |

### Python API

```python
joint = asm.Joints.newObject("Assembly::JointObject", "TimingBelt")
joint.JointType = "Belt"
joint.Reference1 = (doc.getObject("MotorPulley"), ["Face2"])
joint.Reference2 = (doc.getObject("DrivenPulley"), ["Face2"])
joint.Radius1 = 15.0
joint.Radius2 = 45.0   # 3:1 reduction, same direction
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Coupled joint without underlying kinematic joints"
    A Rack and Pinion joint requires the pinion to already have a Revolute
    joint and the rack to already have a Slider joint. Adding the coupled
    joint first will either fail or produce incorrect results. Always add
    the kinematic joints first.

!!! warning "Gears vs Belt direction confusion"
    External spur gears reverse direction — use Gears. Belt and chain drives
    preserve direction — use Belt. Choosing the wrong one produces a mechanism
    that rotates the wrong way in simulation.

!!! warning "Pitch radius vs module"
    FreeCAD takes pitch circle radius (in mm), not module or diametral pitch.
    If you know the module (m) and tooth count (z), the pitch radius is:
    R = m × z / 2. Entering the wrong radius gives the wrong gear ratio.

!!! warning "Coupled joints do not add geometric constraints"
    A Gears joint does not physically constrain the two gears to mesh — it
    only adds the rotational ratio. The gears must be correctly positioned by
    their individual Revolute joints and any distance/coincident constraints.
    The coupled joint purely governs how their angles change relative to each
    other during simulation.

---

## See also

- [Joints — Kinematic](joints-kinematic.md) — the underlying joints that coupled joints build on
- [Joints — Geometric](joints-geometric.md) — distance, parallel, perpendicular, angle
- Solve Assembly — run the solver
- Simulation — drive a joint through a range of motion to animate the mechanism
