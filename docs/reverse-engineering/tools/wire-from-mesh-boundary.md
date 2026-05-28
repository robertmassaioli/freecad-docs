# Wire From Mesh Boundary

> **In one sentence:** Extract the boundary loops of a mesh as Part wires or
> a planar face that can be used in subsequent CAD operations.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Segmentation → Wire From Mesh Boundary…  
**Shortcut:** none

Finds all boundary edge loops of a mesh (edges that have only one adjacent
face — the perimeter of any holes or the outer edge of an open mesh) and
creates a `Part::Feature`. If the loops are co-planar, the result is a face;
otherwise it is a compound of wires.

---

## Intuition

An open mesh has edges on its perimeter — like the border of a cookie cutter
shape. Wire From Mesh Boundary traces all those border edges and converts
them into a proper CAD wire (a connected chain of edges) or a face. You can
then use that wire in sketches, as a sweep path, or as a boundary edge for
surface filling.

---

## When to use it

- You have an open mesh (e.g. only the top surface of a scanned part) and
  want to extract the boundary profile as a CAD curve for modelling.
- You need a closing face to make a mesh watertight for FEM or 3-D printing.
- You want to reference the mesh perimeter in a Sketcher sketch.

---

## When NOT to use it

- **Watertight, closed meshes** — a closed mesh has no boundary edges; the
  tool will create nothing or report no boundaries found.
- **When you need the full mesh converted to a solid** — use Mesh workbench →
  Convert Mesh to Solid or Part workbench → Shape from Mesh.

---

## Step-by-step

1. Select one or more `Mesh::Feature` objects.
2. Choose **Reverse Engineering → Segmentation → Wire From Mesh Boundary…**.
3. FreeCAD creates a `Part::Feature`:
   - If all boundary loops are co-planar: a face is created.
   - Otherwise: a compound of wires is created.

---

## Parameters

This tool has no parameter dialog.

### Result types

| Condition | Result |
|-----------|--------|
| Boundary loops are co-planar | `Part::Feature` with a face (filled) |
| Boundary loops are non-planar | `Part::Feature` with a compound of wires |
| No boundary edges found | Nothing created |

---

## Common mistakes and pitfalls

!!! warning "Produces wires instead of expected face"
    **Cause:** The mesh boundary is not planar — it curves in 3-D space,
    so a flat face cannot fill it.  
    **Fix:** Use the resulting wires as boundary curves for the Surface
    workbench's [Filling](../../surface/tools/filling.md) tool.

!!! warning "No result produced for a closed mesh"
    **Cause:** A watertight mesh has no boundary edges — there is nothing to
    extract.  
    **Fix:** Use [Poisson Reconstruction](poisson-reconstruction.md) or the
    Mesh workbench's cut and segmentation tools to create an open mesh first.

---

## Python API

```python
# Wire From Mesh Boundary is command-driven:
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Reen_MeshBoundary', 0)")

# Access the result:
import FreeCAD as App
wire_feat = App.ActiveDocument.ActiveObject
print(type(wire_feat.Shape))  # Part.Face or Part.Compound
```

---

## See also

- [Mesh Segmentation](mesh-segmentation.md) — segment the mesh before extracting boundaries
- Surface workbench [Filling](../../surface/tools/filling.md) — fill non-planar wire boundaries
- [Reverse Engineering Workbench](../index.md) — workbench overview
