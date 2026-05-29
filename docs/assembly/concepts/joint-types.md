# Joint Types and Degrees of Freedom

> **In one sentence:** Every Assembly joint removes a specific number of
> degrees of freedom (DOF) from a pair of parts — understanding which joint
> removes which DOF lets you choose the right joint and diagnose solver
> errors when a mechanism is over- or under-constrained.

---

## Degrees of freedom in 3-D

A rigid body floating freely in 3-D space has **6 degrees of freedom**:

| DOF | Type | Axis |
|-----|------|------|
| Tx | Translation | Along X |
| Ty | Translation | Along Y |
| Tz | Translation | Along Z |
| Rx | Rotation | About X |
| Ry | Rotation | About Y |
| Rz | Rotation | About Z |

A joint between two parts removes some of those 6 DOF, leaving the
remaining DOF as the joint's allowed motion. For a mechanism to be
**fully constrained** the total DOF removed must equal the total DOF
present: every part contributes 6 DOF, every joint removes some.

---

## Grounding

Before joints can constrain *where* parts are in space, the solver needs
at least one fixed reference point. That is what
[Toggle Grounded](../tools/grounded.md) provides — it removes all 6 DOF
from one part, anchoring it to the assembly origin.

!!! warning "Always ground one part before adding joints"
    Without a grounded part, all joint equations are relative. The solver
    can satisfy "A is 10 mm from B" without knowing where in space either
    A or B is — the entire mechanism floats. Ground the chassis, base
    plate, or frame first.

---

## Joint reference table

### Simple joints (new geometric constraints)

| Joint | DOF removed | DOF remaining | Allowed motion |
|-------|------------|--------------|----------------|
| [Fixed](../tools/joint-fixed.md) | 6 | 0 | None — parts are locked together |
| [Revolute](../tools/joint-revolute.md) | 5 | 1 | Rotation about one axis |
| [Slider](../tools/joint-slider.md) | 5 | 1 | Translation along one axis |
| [Cylindrical](../tools/joint-cylindrical.md) | 4 | 2 | Rotation + translation along one axis |
| [Ball](../tools/joint-ball.md) | 3 | 3 | All rotations about one point (no translation) |
| [Parallel](../tools/joint-parallel.md) | 2 | 4 | 1 spin + 3 translations |
| [Perpendicular](../tools/joint-perpendicular.md) | 1 | 5 | 2 rotations + 3 translations |
| [Angle](../tools/joint-angle.md) | 1 | 5 | 2 rotations + 3 translations |
| [Distance](../tools/joint-distance.md) | 1 | 5 | All except the constrained gap/separation |

### Coupled joints (kinematic ratios, no new constraints)

| Joint | What it couples | How |
|-------|-----------------|-----|
| [Gears](../tools/joint-gears.md) | Two Revolute joints | ω₂/ω₁ = −R₁/R₂ (external: direction reverses) |
| [Belt](../tools/joint-belt.md) | Two Revolute joints | ω₂/ω₁ = +R₁/R₂ (same direction) |
| [Rack and Pinion](../tools/joint-rack-pinion.md) | Revolute + Slider | Translation = 2π × pitch_radius per revolution |
| [Screw](../tools/joint-screw.md) | Revolute + Slider (same axis) | Advance = pitch per revolution |

Coupled joints **do not add new geometric constraints** — they add a
kinematic ratio between two DOF that already exist. Use them in
addition to the base joints (Revolute, Slider) that define the motion axes.

---

## Choosing the right joint

### "This part turns on this shaft"  →  Revolute

One rotation DOF. The classic hinge or pin joint. Optionally add
`EnableLimitAngle` to constrain the range.

### "This part slides along this rail"  →  Slider

One translation DOF. A piston in a cylinder, a drawer on a drawer slide.
Optionally add `EnableLimitDistance` to set travel limits.

### "This part can slide *and* spin on this shaft"  →  Cylindrical

Two DOF on one axis. Think of a loose pen cap: it can rotate around the
barrel and also slide along it. Use Screw joint on top if rotation and
translation are coupled.

### "These two shafts are always parallel but I don't care about their angle"  →  Parallel

Fixes two rotational DOF (the two tilts), leaving spin and all three
translations free. Useful for constraining gear shafts to remain parallel.

### "These two faces must always be perpendicular"  →  Perpendicular

Removes only 1 DOF — the one that would break the 90° relationship.
Good for T-slot or rail mechanisms where one direction is locked
orthogonal to another.

### "I need to constrain a gap or clearance"  →  Distance

Does not prescribe a motion type — just enforces that a point, edge, or
face is separated from another by a specified scalar distance.

---

## Over- and under-constraining

**Under-constrained** — the solver has more DOF than constraints. Parts
float to an arbitrary position. Symptoms: parts "teleport" on recompute,
the solver reports residual DOF. Fix: add joints or ground another part.

**Over-constrained** — more joint equations than DOF. The solver cannot
satisfy all constraints simultaneously. Symptoms: solver error,
"Inconsistent constraints" message, or parts snap to a position that
satisfies some joints but violates others.
Fix: remove one of the conflicting joints.

### Common over-constraint traps

| Trap | Why it over-constrains | Fix |
|------|----------------------|-----|
| Fixed joint + another joint between the same two parts | Fixed already removes all 6 DOF | Delete the Fixed joint |
| Two Revolute joints on the same axis | Each removes 5 DOF including all off-axis rotation | Use one Revolute only |
| Parallel + Angle on the same axis pair | Both target the same rotational DOF | Use Angle alone (it implies parallel) |
| Gears/Belt/Screw added without the underlying Revolute/Slider joints | Coupled joints need the base joints to exist | Add the Revolute and Slider joints first |

---

## Limit properties

The Revolute and Slider joints support motion limits. These properties
appear in the task panel and in the joint's Properties view:

**Revolute joint limits**

| Property | Default | Meaning |
|----------|---------|---------|
| `EnableLimitAngle` | `false` | Enable/disable the angular travel limits |
| `LimitAngleMin` | −180° | Minimum allowed rotation from reference |
| `LimitAngleMax` | 180° | Maximum allowed rotation from reference |

**Slider joint limits**

| Property | Default | Meaning |
|----------|---------|---------|
| `EnableLimitDistance` | `false` | Enable/disable the linear travel limits |
| `LimitDistanceMin` | −∞ | Minimum allowed displacement from reference |
| `LimitDistanceMax` | +∞ | Maximum allowed displacement from reference |

Limits are enforced during [Simulation](../tools/simulation.md) but do
not block the static solver — you can still position parts outside limits
when not running a simulation.

---

## Links vs copies in assemblies

When you [Insert a Component](../tools/insert-component.md), FreeCAD can
create a *link* (the default) or a *copy*:

| | Link | Copy |
|--|------|------|
| Shares geometry with source | Yes | No |
| Updates if source changes | Yes | No |
| Can have independent joint constraints | Yes | Yes |
| File size | Smaller | Larger |

Links are preferred for repeated components (bolts, identical brackets).
Each link can have its own independent set of joints in the assembly.

---

## Python API

```python
import FreeCAD

doc = FreeCAD.ActiveDocument
assembly = doc.getObject("Assembly")

# Create a Revolute joint programmatically
joint = doc.addObject("Assembly::JointRevolute", "HingeJoint")
assembly.addObject(joint)

# Set joint type (alternative: use the specific subclass above)
joint.JointType = "Revolute"

# Reference the two parts and their features
joint.Reference1 = (doc.getObject("Part1"), ["Face3"])
joint.Reference2 = (doc.getObject("Part2"), ["Face7"])

# Enable angular limits
joint.EnableLimitAngle = True
joint.LimitAngleMin = -90.0  # degrees
joint.LimitAngleMax = 90.0   # degrees

doc.recompute()
```

---

## See also

- [New Assembly](../tools/new-assembly.md) — creating the assembly container
- [Insert Component](../tools/insert-component.md) — adding parts to an assembly
- [Toggle Grounded](../tools/grounded.md) — anchoring the reference part
- [Solve Assembly](../tools/solve.md) — running the constraint solver
- [Simulation](../tools/simulation.md) — animating a kinematic mechanism
- [Assembly Workbench](../index.md) — workbench overview
