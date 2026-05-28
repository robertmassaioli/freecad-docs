# Hull

> **In one sentence:** Compute the convex hull of all selected objects using
> OpenSCAD's CGAL engine, producing the smallest convex solid that contains
> every input shape.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Hull  
**Requires:** OpenSCAD binary installed  
**Shortcut:** none

Passes all selected objects to OpenSCAD as a `hull()` call and imports the
result as a `Part` object. All input objects are hidden. The convex hull is
the tightest convex bounding solid around all the inputs.

---

## Intuition

Wrap a rubber band around several objects lying on a table — the shape the
rubber band makes is the 2-D convex hull. Hull does the same thing in 3-D:
it finds the smallest convex solid that contains all the selected objects. No
concavities, no overhangs — just the outer convex skin.

---

## When to use it

- You need a convex bounding solid for interference or clearance checking.
- You want to create a convex approximation of a complex shape for simulation.
- You are using the Hull-then-Minkowski pattern to create rounded offset shapes.

---

## When NOT to use it

- **Without OpenSCAD installed** — requires the OpenSCAD binary.
- **When you need non-convex bounds** — use Part workbench bounding box tools.

---

## Step-by-step

1. Select two or more objects.
2. Choose **OpenSCAD → Hull**.
3. FreeCAD hides the inputs and creates a `Part` object containing the hull.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_Hull', 0)")
```

---

## See also

- [Minkowski Sum](minkowski-sum.md) — Minkowski sum of objects
- [Mesh Boolean](mesh-boolean.md) — includes Hull as an operation option
- [OpenSCAD Workbench](../index.md) — workbench overview
