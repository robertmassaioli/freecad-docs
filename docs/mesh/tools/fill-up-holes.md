# Fill Up Holes

> **In one sentence:** Automatically detect and fill all open boundary
> loops (holes) in the mesh with new triangles.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Fill-up Holes…  
**Command:** `Mesh_FillupHoles`  
**Shortcut:** none

Automatically detects all open boundary loops in the mesh and fills them with
new triangles. A size filter lets you skip intentionally open boundaries (e.g.,
the open base of a vase) by limiting fills to holes with fewer than N boundary
edges.

---

## Intuition

After importing a scan mesh, there are often small holes from scan occlusions
or defects. Fill Up Holes closes all of them at once. For a mesh to be
3-D printable (watertight) all holes must be closed — this tool does that
automatically.

For more control over which specific holes to fill, use
[Fill Hole Interactively](fill-hole-interactively.md).

---

## When to use it

- After importing a scan mesh with multiple small holes.
- After [Evaluate and Repair](evaluate-and-repair.md) reports open boundaries.
- As part of a batch repair workflow: Import → Evaluate & Repair → Fill Holes
  → Evaluate Solid → Export.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Fill-up Holes…**.
3. Set **Max hole size** (number of boundary edges).
4. Click **OK**. All holes with ≤ that many boundary edges are filled.
5. Run [Evaluate Solid](evaluate-solid.md) to confirm the mesh is now closed.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Max hole size | Fill only holes with fewer than this many boundary edges. Set high (e.g. 1000) to fill all holes. |

---

## Common mistakes and pitfalls

!!! warning "The fill algorithm is flat (fan triangulation)"
    FreeCAD uses a simple fan triangulation to fill holes: a centre point
    plus triangles to each boundary edge. This produces a flat or slightly
    concave patch regardless of surrounding curvature. For curved surfaces,
    inspect and manually refine large filled areas.

!!! warning "Large holes may be intentional"
    A vase model has an intentionally open base. Setting Max hole size too
    high will fill it. Use [Fill Hole Interactively](fill-hole-interactively.md)
    for selective filling.

---

## Python API

```python
import FreeCAD as App

mesh_obj = App.ActiveDocument.getObject("Mesh")
mesh = mesh_obj.Mesh.copy()
mesh.fillupHoles(10)   # fill holes with up to 10 boundary edges
mesh_obj.Mesh = mesh
App.ActiveDocument.recompute()
```

---

## See also

- [Fill Hole Interactively](fill-hole-interactively.md) — fill specific holes selectively
- [Evaluate and Repair](evaluate-and-repair.md) — find holes before filling
- [Evaluate Solid](evaluate-solid.md) — verify the mesh is closed after filling
- [Mesh Workbench](../index.md) — workbench overview
