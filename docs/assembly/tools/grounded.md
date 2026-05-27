# Toggle Grounded

> **In one sentence:** Fix a part's position in the assembly frame so the
> solver has an immovable reference point — the foundation every assembly needs
> before adding joints.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Toggle Grounded  
**Shortcut:** `G`

---

## Intuition

The constraint solver works by satisfying all joint equations simultaneously.
To do that it needs at least one part whose position is *given* rather than
*solved*. That is what grounding does: it removes all six degrees of freedom
(3 translation + 3 rotation) from one part, anchoring it to the assembly
coordinate origin.

Without a grounded part:

- Every joint equation is relative — "A is 10 mm away from B" does not tell
  the solver where in space A or B are.
- The entire mechanism floats; the solver may converge to an arbitrary
  position or report under-constrainment.

With a grounded part, the solver has a fixed frame and can calculate where
every other part must be to satisfy all joints.

**Choose the base, chassis, or frame** — the part that everything else is
bolted or sliding against. Grounding it reflects the physical reality that
this part is bolted to the bench, welded to the wall, or otherwise immovable.

---

## When to use it

- **Always** — every assembly needs exactly one grounded part before adding
  joints.
- **Re-grounding** — if you restructure an assembly and the original grounded
  part changes role, use Toggle Grounded to un-ground it and ground the new
  reference part.
- **Debugging a floating mechanism** — if Solve Assembly reports
  under-constrainment even though you have added joints, check that exactly
  one part is grounded.

---

## Step-by-step

1. Select one part in the model tree or 3-D view.
2. **Assembly → Toggle Grounded** (or press `G`).
3. A padlock icon appears on the part in the model tree.
4. The part's position is now fixed. Run **Solve Assembly** — the solver will
   compute positions for all other parts relative to this one.

To unground the part, select it and press `G` again. The padlock icon
disappears and the part becomes free.

---

## How grounding is stored

Grounding is stored as a `Assembly::GroundedJoint` object inside the
assembly's `Joints` container. It is a special zero-DOF joint that references
only one part (rather than the usual two). You can see it in the model tree
under `Joints`.

Deleting the `GroundedJoint` object has the same effect as using Toggle
Grounded to unground the part.

---

## Multiple grounded parts

Technically, you can ground more than one part. This is valid and sometimes
useful (e.g. two separate sub-assemblies in one document where each
sub-assembly has its own fixed frame). However, for a single mechanism with
parts that must move relative to each other, grounding more than one part
over-constrains the system and the solver will report a conflict.

---

## Common mistakes and pitfalls

!!! warning "Forgetting to ground before solving"
    If Solve Assembly completes but all parts remain at the origin or cluster
    together, check that exactly one part is grounded. The padlock icon in
    the model tree makes this easy to spot.

!!! warning "Grounding the wrong part"
    Ground the mechanically fixed reference part. If you accidentally ground
    a moving part (e.g. a slider block), all other parts will be positioned
    relative to that block — which may produce a valid but counter-intuitive
    result. Toggle Grounded to swap the reference.

!!! warning "Deleting a grounded part"
    Deleting a grounded part also deletes its `GroundedJoint`. Any joints
    that referenced the deleted part will become invalid. Re-ground another
    part before re-solving.

---

## Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

doc = App.newDocument()

# Create assembly
asm = doc.addObject("Assembly::AssemblyObject", "Assembly")
doc.recompute()

# Ground a part programmatically
# (Select the part in the tree first, then call the command)
Gui.doCommand('import CommandCreateJoint')
# Or add a GroundedJoint directly:
grounded = asm.Joints.newObject("Assembly::GroundedJoint", "GroundedJoint")
grounded.ObjectToGround = doc.getObject("Plate")
doc.recompute()
```

---

## See also

- [Assembly Setup](create-assembly.md) — create the assembly and insert parts first
- [Joints — Kinematic](joints-kinematic.md) — constrain relative motion between parts after grounding
- [Solve Assembly](solve.md) — run the solver once grounding and joints are in place
