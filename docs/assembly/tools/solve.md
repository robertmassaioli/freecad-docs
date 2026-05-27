# Solve Assembly

> **In one sentence:** Run the constraint solver to move every part into the
> position that satisfies all joints simultaneously — the step that turns a
> collection of inserted parts into an assembled mechanism.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Solve Assembly  
**Shortcut:** none

---

## Intuition

Adding joints to an assembly declares *relationships* between parts — "this
shaft must spin inside this bore" — but parts do not automatically move to
satisfy those relationships just because you added the joint. Solve Assembly
is the explicit command that hands your joint declarations to the built-in
constraint solver and asks it to calculate a consistent position for every
part.

Think of the solver as a simultaneous equation solver: every joint adds one
or more equations (e.g. "axis 1 and axis 2 must be co-linear"), and the
solver finds values for all part placements that satisfy all equations at
once. If the equations are consistent and the system is exactly constrained,
one unique solution exists and the solver finds it.

**When to run it:**

- After adding joints.
- After moving a part in the 3-D view (the move is tentative; solving
  snaps everything back into joint-consistent positions).
- After changing a joint parameter (offset, angle, distance).
- Before running a simulation or creating an exploded view.

---

## Running the solver

1. Make sure the correct assembly is active (double-click it in the model
   tree if needed).
2. Choose **Assembly → Solve Assembly** or click the Solve button in the
   Assembly toolbar.
3. FreeCAD computes positions for all parts and moves them.
4. The **Report** panel (View → Panels → Report view) shows the outcome.

---

## Solver outcomes

### Success

All parts move to their joint-consistent positions. The Report view shows no
errors. The assembly is ready for animation, exploded view, or BOM export.

### Under-constrained

The solver reports that the system has residual DOF — some parts are still
free to move. This is not always an error:

- A mechanism that is **intentionally mobile** (e.g. a four-bar linkage
  being driven by a simulation) is correctly under-constrained at 1 DOF.
- A part that truly needs another joint is under-constrained by mistake.

To diagnose, count DOF manually:

```
Free DOF = (non-grounded parts × 6) − (sum of DOF removed by all joints)
```

If Free DOF > 0, add joints or ground additional parts.

### Over-constrained

The solver reports a conflict — the joint equations are inconsistent. Common
causes:

- Two joints remove the same DOF (e.g. a Revolute and a Fixed joint between
  the same pair of parts).
- Closing a kinematic loop with one too many constraints (a four-bar linkage
  with five joints instead of four).

To fix, identify and remove redundant joints. A single Revolute joint already
removes 5 DOF; a Fixed removes 6 DOF. If both are applied between the same
parts, one is redundant.

### Solver failure (no convergence)

The solver attempted to find a solution but failed to converge. Common causes:

- The initial part positions are so far from the solution that the solver
  cannot find its way there. Try manually moving parts closer to the desired
  position before solving.
- Contradictory joint definitions (e.g. a Slider joint says "translate along
  X" while a Perpendicular joint says "X-axis must be perpendicular to
  itself").
- Numerical singularities in nearly-degenerate geometry.

---

## Fixing common solver problems

### Parts cluster at the origin

One or more parts are still floating at the origin after solving. Usually
means no joint connects those parts to the grounded part — they are isolated
from the constraint network. Check the model tree: every non-grounded part
must be connected by a chain of joints back to the grounded part.

### Solver positions the part on the wrong side

Joint references have orientation. If the solver places a part mirrored or
on the unexpected side:

- Try selecting a different face on one of the parts (one that has the
  normal pointing the other way).
- Change the sign of an offset parameter.
- Use the joint's **Flip** option in the task panel (if available).

### Slow convergence on large assemblies

The constraint solver complexity grows with part count. For large assemblies:

- Use sub-assemblies — solve each sub-assembly separately, then assemble
  sub-assemblies with fewer inter-unit joints.
- Fix parts that are rigid internally with a single Fixed joint rather than
  multiple individual joints.
- Remove redundant joints to simplify the system.

---

## Python API

Solve Assembly can be triggered programmatically:

```python
import FreeCAD as App
import FreeCADGui as Gui

# Trigger via GUI command (most reliable — follows active assembly)
Gui.doCommand('import CommandSolveAssembly')
Gui.doCommand('CommandSolveAssembly.CommandSolveAssembly().Activated()')

# Or use the solver directly on a known assembly
doc = App.ActiveDocument
asm = doc.getObject("Assembly")

import UtilsAssembly
UtilsAssembly.solveIfAllowed(asm)
```

For scripted parameter sweeps (changing a joint offset and re-solving in a
loop):

```python
import FreeCAD as App
import UtilsAssembly

doc = App.ActiveDocument
asm = doc.getObject("Assembly")
slider_joint = doc.getObject("PistonSlider")

for offset_mm in range(0, 50, 5):
    slider_joint.Offset = offset_mm
    doc.recompute()
    UtilsAssembly.solveIfAllowed(asm)
    # record results here
```

---

## See also

- [Toggle Grounded](grounded.md) — every assembly needs a grounded part before solving
- [Joints — Kinematic](joints-kinematic.md) — the joints the solver processes
- [Joints — Geometric](joints-geometric.md) — geometric constraints processed by the solver
- [Simulation](simulation.md) — drive joints through a range and record the motion
