# Convert Edges to Faces

> **In one sentence:** Build planar faces from closed edge loops in selected
> `Part::Feature` objects, recovering 2-D geometry as proper face objects.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Convert Edges to Faces  
**Shortcut:** none

Analyses the edges of selected Part objects, detects closed edge loops that
enclose a planar region, and constructs faces from them. Used to recover 2-D
planar geometry (a set of edges that enclose a region but have no face) as
proper `Part::Face` objects suitable for extrusion or further CAD operations.

---

## Intuition

Sometimes imported geometry (from DXF, IGES, or badly-exported STEP) contains
only edge loops with no faces — like a wire frame drawing rather than a solid.
Convert Edges to Faces fills in those loops with flat faces, turning the wire
frame into proper surface geometry.

---

## When to use it

- An imported DXF or IGES file contains only edges (no faces) and you need
  faces to extrude or work with the geometry.
- A boolean operation left behind edge loops that are now isolated from any
  face and you need to recover them as faces.

---

## When NOT to use it

- **Non-planar edge loops** — the tool can only create flat (planar) faces
  from closed loops that lie in a single plane.

---

## Step-by-step

1. Select one or more `Part::Feature` objects whose edges form closed planar loops.
2. Choose **OpenSCAD → Convert Edges to Faces**.
3. FreeCAD creates a new `Part::Feature` with the faces filled in.

---

## Parameters

No task panel — operates automatically on selected objects.

---

## Common mistakes and pitfalls

!!! warning "Non-planar loops produce no faces"
    **Cause:** The algorithm can only fill loops that lie in a single plane.
    3-D loops (like the edges of a curved face) cannot be converted.  
    **Fix:** Use the Surface workbench [Filling](../../surface/tools/filling.md)
    tool to fill non-planar loops.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_Edgestofaces', 0)")
```

---

## See also

- [OpenSCAD Workbench](../index.md) — workbench overview
