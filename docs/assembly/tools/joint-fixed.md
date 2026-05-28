# Fixed Joint

> **In one sentence:** Lock two parts together with no relative motion — the equivalent of welding or gluing them.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Joints → Fixed  
**Shortcut:** `F`  
**DOF removed:** 6 | **DOF remaining:** 0

A Fixed joint locks two parts together completely. All six degrees of freedom (3 translations + 3 rotations) between the two parts are removed. The solver moves Part 2 so its selected geometry aligns with Part 1's selected geometry, then freezes all relative motion.

---

## Intuition

Think of a Fixed joint as a weld bead or a structural adhesive bond. Once applied, the two parts become a single rigid unit — they move together as if they were one piece. No sliding, no spinning, no tilting.

The solver uses the selected geometry to determine the *alignment* before locking: if you select two coplanar faces, the parts are brought into contact and locked. If you select two vertices, the vertices are brought to the same point and locked.

---

## When to use it

- Bolted flanges, press-fit components, or glued surfaces that must not move.
- Sub-assemblies that are internally rigid — fix the sub-assembly to its mounting interface.
- Temporarily locking a part during assembly setup to simplify the solver while adding other joints.
- Any component that is permanently attached and has no relative motion.

## When NOT to use it

- **Don't use Fixed when parts have relative motion** — use [Revolute](joint-revolute.md) for rotation, [Slider](joint-slider.md) for translation, etc.
- **Don't use Fixed in addition to another joint between the same pair of parts** — the Fixed joint already removes all 6 DOF, making any other joint between those parts redundant and over-constraining the system.

---

## Step-by-step

1. Press `F` or choose **Assembly → Joints → Fixed**.
2. Click a face, edge, or vertex on **Part 1** (the reference part).
3. Click the corresponding feature on **Part 2** (the part to align and fix).
4. Click **OK** in the task panel.

The solver moves Part 2 so its selected feature aligns with Part 1's feature, then removes all relative motion. Run [Solve Assembly](solve.md) to apply.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Reference 1 | Link | Geometry on Part 1 (face, edge, or vertex) |
| Reference 2 | Link | Geometry on Part 2 |

Fixed joints have no offset or rotation parameters — the parts are locked at their alignment position without any additional offset.

---

## Python API

### Minimal example

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

### Resulting object properties

| Property | Type | Description |
|----------|------|-------------|
| `JointType` | `App::PropertyString` | Always `"Fixed"` |
| `Reference1` | `App::PropertyXLinkSub` | Geometry on Part 1 |
| `Reference2` | `App::PropertyXLinkSub` | Geometry on Part 2 |

---

## Common mistakes and pitfalls

!!! warning "Fixed + another joint between the same parts over-constrains"
    A Fixed joint removes all 6 DOF. Adding any other joint (Revolute, Slider,
    Parallel, …) between the same two parts attempts to remove DOF that are
    already removed. The solver will report a conflict. Use only one joint type
    per part pair when Fixed is involved.

!!! warning "Using Fixed for a 'not-yet-jointed' part"
    It is tempting to Fix a newly inserted part in place before deciding on the
    right joint type. That is fine for setup, but remember to delete the
    temporary Fixed joint before adding the correct kinematic joint — two
    joints between the same parts over-constrain the system.

---

## See also

- [Toggle Grounded](grounded.md) — fix one part to the assembly frame (not to another part)
- [Revolute Joint](joint-revolute.md) — allow rotation between two parts
- [Slider Joint](joint-slider.md) — allow translation between two parts
- [Solve Assembly](solve.md) — apply all joints
