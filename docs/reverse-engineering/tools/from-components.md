# From Components

> **In one sentence:** Create one mesh segment per disconnected component of
> the input mesh without any analysis or painting.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Segmentation → From Components  
**Shortcut:** none

Splits a mesh into individual objects based on existing topological
disconnection — each connected component (a set of faces all reachable
from each other through shared edges) becomes a separate `Mesh::Feature`
in a "Segments" group.

---

## Intuition

A mesh is "connected" when every face can reach every other face by walking
across shared edges. If you scan two separate bolts and import them as one
mesh file, there are two connected components (the two bolts) even though
both live in the same object. From Components splits them back into
individual objects.

No geometry analysis is involved — the tool simply maps the topology.

---

## When to use it

- You imported a mesh file (STL, OBJ) that contains multiple objects merged
  into one, and you want them as separate objects.
- You ran Mesh workbench → Split by Components and now want the pieces as
  Reverse Engineering segments.
- You want to fit separate analytic surfaces to parts of a mesh that are
  already physically disconnected.

---

## When NOT to use it

- **Connected mesh** — if the entire mesh is one component, From Components
  creates a group with a single member (the original mesh). Use
  [Mesh Segmentation](mesh-segmentation.md) or
  [Manual Segmentation](manual-segmentation.md) to subdivide it.
- **When you need type classification** — From Components doesn't label
  regions as plane/cylinder/sphere. Use Mesh Segmentation for that.

---

## Step-by-step

1. Select one or more `Mesh::Feature` objects.
2. Choose **Reverse Engineering → Segmentation → From Components**.
3. FreeCAD creates a "Segments \<mesh name\>" group containing one
   `Mesh::Feature` per disconnected component.

---

## Parameters

This tool has no parameters — it is fully determined by the mesh topology.

---

## Common mistakes and pitfalls

!!! warning "Creates only one segment — mesh was already connected"
    **Cause:** The mesh is topologically one connected component even though
    it looks like multiple objects.  
    **Fix:** Use [Mesh Segmentation](mesh-segmentation.md) or
    [Manual Segmentation](manual-segmentation.md) to split by geometry.

---

## Python API

```python
# From Components is command-driven:
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Reen_SegmentationFromComponents', 0)")
```

---

## See also

- [Mesh Segmentation](mesh-segmentation.md) — auto-split by surface type
- [Manual Segmentation](manual-segmentation.md) — paint regions interactively
- [Reverse Engineering Workbench](../index.md) — workbench overview
