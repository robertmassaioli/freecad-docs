# Ball Joint

> **In one sentence:** Fix the position of a point on one part to a point on another while allowing completely free rotation in any direction — the joint for spherical bearings and ball-and-socket connections.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Ball  
**Shortcut:** `B`  
**DOF removed:** 3 | **DOF remaining:** 3 (all rotations about one point)

A Ball joint locks the three translational DOF (the ball cannot leave the socket) while leaving all three rotational DOF free (the ball can point in any direction). The selected geometry defines the point where the two parts are pinned together.

---

## Intuition

Picture a ball-and-socket joint — the kind used in the human hip, a trailer hitch, or a spherical bearing. The ball is locked inside the socket: it cannot translate, cannot escape. But once inside, it can rotate freely in any direction — tilt forward, backward, sideways, and spin about any axis through the ball centre.

A Ball joint models this: three translations removed, three rotations free.

---

## When to use it

- Ball-and-socket joints (shoulder joint, trailer hitch, ball linkages).
- Rod ends on steering linkages and suspension arms — the rod can pivot in any direction at the end.
- Spherical bearings where the inner race can tilt relative to the outer.
- Universal joint simulation (combine with a second Ball joint at the other end of a link).

## When NOT to use it

- **Don't use Ball when only rotation about one axis is needed** — use [Revolute](joint-revolute.md). Ball leaves 3 rotational DOF free; Revolute leaves only 1.
- **Don't use Ball if the point must also translate** — Ball locks all translation. If the point can move in a plane, combine with other joints.

---

## Step-by-step

1. Press `B` or choose **Assembly → Joints → Ball**.
2. Click a **vertex, spherical face, or the centre of a circular edge** on Part 1 — the socket point.
3. Click the corresponding point on Part 2 — the ball centre.
4. Click **OK**.

The solver moves Part 2 so its ball centre coincides with Part 1's socket point. Part 2 can then rotate freely about that point.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Reference 1 | Link | Point or spherical face on Part 1 (the socket) |
| Reference 2 | Link | Point or spherical face on Part 2 (the ball) |

Ball joints have no offset or angle parameters — the two points are brought to coincidence and locked there.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.ActiveDocument
asm = doc.getObject("Assembly")

joint = asm.Joints.newObject("Assembly::JointObject", "SphericBearing")
joint.JointType = "Ball"
joint.Reference1 = (doc.getObject("Housing"), ["Vertex3"])
joint.Reference2 = (doc.getObject("Rod"), ["Vertex1"])
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Ball joint leaves 3 rotational DOF free"
    A Ball joint intentionally leaves all three rotational DOF unconstrained.
    For a mechanism with a single link rod connected by two Ball joints, the
    rod still has one free rotational DOF (spin about its own axis). The solver
    will report under-constrainment unless this is accounted for — either by
    adding a constraint on that spin or accepting it as a genuinely free DOF.

!!! warning "Selecting a face instead of a vertex or spherical face"
    Selecting a planar face gives the solver a normal direction, not a point.
    Ball joints require a point reference. Use a vertex or a spherical face
    (which defines its centre point).

!!! warning "Ball vs Revolute for hinge-like connections"
    A Ball joint gives too much freedom for a simple hinge. A hinge should
    use Revolute (1 rotational DOF). Use Ball only when multi-directional
    rotation is physically correct.

---

## See also

- [Revolute Joint](joint-revolute.md) — rotation about one axis only
- [Fixed Joint](joint-fixed.md) — lock all motion
- [Solve Assembly](solve.md) — apply joints and check DOF count
- [Simulation](simulation.md) — animate mechanisms with Ball joints
