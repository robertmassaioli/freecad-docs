# Convert to Solid

> **In one sentence:** Close a Part shell or surface shape into a solid by
> stitching its boundary faces — requires the shell to be watertight.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Convert to Solid  
**Shortcut:** none

Takes a Part shell (an open or closed surface) and attempts to create a solid
from it. Requires the input to be a closed shell — every edge must be shared
by exactly two faces (no boundary edges / gaps).

---

## Intuition

A shell is a collection of faces — like the walls of a building with no floor
and no roof (or holes in the walls). A solid is a completely enclosed volume.
Convert to Solid checks whether the shell's faces completely enclose a volume,
and if so, declares it a solid that Part tools can work with for Boolean
operations, mass properties, and PartDesign features.

This is the second step in the standard **STL → solid repair pipeline**:

```
STL file → Shape from Mesh → Convert to Solid → Refine Shape → usable solid
```

---

## When to use it

- After [Shape from Mesh](shape-from-mesh.md), to obtain a usable solid from
  a watertight mesh.
- When a STEP or IGES import results in a shell rather than a solid.
- When [Shape Builder](shape-builder.md) assembled a closed shell from faces.

## When NOT to use it

- **Don't use it on open shells** — if the mesh/shell has any gaps or boundary
  edges, the conversion will fail. Repair the mesh first.
- **Don't use it on meshes directly** — convert mesh to shell with
  [Shape from Mesh](shape-from-mesh.md) first, then use this tool.

---

## Step-by-step

1. Select the shell `Part::Feature` in the model tree.
2. Choose **Part → Convert to Solid**.
3. If the shell is closed, a solid `Part::Feature` appears.
4. Verify with **Part → Check Geometry**.

---

## Parameters

This tool has no task panel. The conversion is attempted immediately.

---

## Common mistakes and pitfalls

!!! warning "Open shell — boundary edges exist"
    If the input shell has any gaps (boundary edges), Convert to Solid fails.
    Check the shape with **Part → Check Geometry** to locate boundary edges,
    then repair the mesh in the Mesh workbench (Mesh → Fill Holes) before
    retrying.

!!! warning "Self-intersecting mesh produces invalid solid"
    A shell with self-intersections will convert but produce an invalid solid
    that fails in Boolean operations. Fix self-intersections in the Mesh
    workbench first.

!!! warning "Shell with inverted normals acts as empty space"
    If the converted solid behaves incorrectly in Booleans (e.g., cutting
    produces no result), the shell normals may be inverted. Use
    [Reverse Shapes](reverse-shapes.md) to flip them before converting.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a closed shell and convert to solid
box_shape = Part.makeBox(10, 10, 10)
shell = box_shape.Shells[0]   # already a closed shell for a box

solid = Part.Solid(shell)
feat = doc.addObject("Part::Feature", "ConvertedSolid")
feat.Shape = solid
doc.recompute()

print(f"Shape type: {feat.Shape.ShapeType}")   # Solid
print(f"Volume: {feat.Shape.Volume:.2f} mm³")
```

---

## See also

- [Shape from Mesh](shape-from-mesh.md) — first step: create a shell from a mesh
- [Refine Shape](refine-shape.md) — clean up co-planar faces after conversion
- [Reverse Shapes](reverse-shapes.md) — fix inverted normals if Boolean results are wrong
- [Check Geometry](check-geometry.md) — verify the resulting solid
- [Part Workbench](../index.md) — workbench overview
