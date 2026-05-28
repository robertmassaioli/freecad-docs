# Mesh Boolean

> **In one sentence:** Perform a union, intersection, or difference on meshes
> using OpenSCAD's CGAL boolean engine as an alternative to OCCT's B-rep booleans.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Mesh Boolean  
**Requires:** OpenSCAD binary installed  
**Shortcut:** none

Converts selected objects to STL meshes, passes them to OpenSCAD's CGAL engine
for the boolean operation, and imports the result back as a `Mesh::Feature`.
Used as a fallback when Part workbench boolean operations fail due to
near-degenerate or self-intersecting geometry.

---

## Intuition

FreeCAD's native boolean operations use OCCT (OpenCASCADE) which is excellent
at exact B-rep booleans but can fail on "difficult" geometry (very thin walls,
near-zero-thickness features, self-intersections). CGAL (the geometry kernel
OpenSCAD uses) approaches booleans differently and can often succeed where OCCT
fails — but it outputs a triangle mesh, not a parametric B-rep.

---

## When to use it

- A Part Boolean fails with "bad result" and you've tried geometry repair.
- You are working primarily with mesh geometry and don't need B-rep output.
- You need any of: Union, Intersection, Difference, Hull, or Minkowski sum
  on mesh input.

---

## When NOT to use it

- **When you need B-rep output** — the result is a mesh, not a solid. It
  cannot be used in Part Design without converting via Part → Shape from Mesh.
- **Without OpenSCAD installed** — this tool requires the OpenSCAD binary.

---

## Step-by-step

1. Select the input objects (meshes or Part solids — solids are converted to
   meshes automatically).
2. Choose **OpenSCAD → Mesh Boolean**.
3. A dialog appears. Select the operation (Union, Intersection, Difference,
   Hull, Minkowski).
4. Click **OK**. The result is imported as a `Mesh::Feature`.

---

## Parameters

| Operation | Description |
|-----------|-------------|
| Union | Combined volume of all inputs |
| Intersection | Volume common to all inputs |
| Difference | First object minus all subsequent objects |
| Hull | Convex hull of all inputs |
| Minkowski | Minkowski sum of all inputs |

---

## Common mistakes and pitfalls

!!! warning "OpenSCAD executable not found"
    **Cause:** Path not configured.  
    **Fix:** Edit → Preferences → OpenSCAD → set the OpenSCAD executable path.

!!! warning "Result is a mesh, not a solid — can't use in Part Design"
    **Cause:** CGAL outputs triangle meshes; the result is a `Mesh::Feature`.  
    **Fix:** Use Part → Shape from Mesh to convert back to a B-rep solid
    before using it in Part Design.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_MeshBoolean', 0)")
```

---

## See also

- [Hull](hull.md) — convex hull shortcut
- [Minkowski Sum](minkowski-sum.md) — Minkowski sum shortcut
- [Add OpenSCAD Element](add-openscad-element.md) — type OpenSCAD code directly
- [OpenSCAD Workbench](../index.md) — workbench overview
