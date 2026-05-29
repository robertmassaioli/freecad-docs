# Shape from Mesh

> **In one sentence:** Wrap a Mesh object as a Part shell shape so it can
> participate in Boolean operations and other Part workbench tools.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Shape from Mesh…  
**Shortcut:** none

Takes a Mesh object (from the Mesh workbench or an imported STL/OBJ) and
creates a `Part::Feature` containing a shell of triangular faces. The result
is not a clean B-rep solid — it is a Part shape whose faces are the original
mesh triangles.

---

## Intuition

FreeCAD has two fundamentally different geometric representations:

- **Meshes** — a cloud of triangles, used by STL files and the Mesh workbench
- **B-rep shapes** — precise boundary-representation geometry used by Part,
  Part Design, and OCCT

These two worlds don't mix directly: you cannot Boolean-cut a mesh with a
Part sphere. Shape from Mesh builds the bridge by wrapping the mesh triangles
in a Part shell, making the mesh accessible to Part tools.

This is typically the first step in the **STL → solid repair pipeline**:

```
STL file → Shape from Mesh → Convert to Solid → Refine Shape → usable solid
```

---

## When to use it

- You imported an STL and need to Boolean-cut it or measure it with Part tools.
- You have a Mesh workbench object and need to convert it to Part geometry.
- You are following the standard mesh-to-solid repair workflow.

## When NOT to use it

- **Don't use it as a final step** — the result is a shell of triangles, not a
  clean solid. Follow up with [Convert to Solid](convert-to-solid.md) and
  optionally [Refine Shape](refine-shape.md).
- **Don't use it if the mesh has gaps** — the sewing tolerance helps bridge
  small gaps, but large holes must be repaired in the Mesh workbench first.

---

## Step-by-step

1. Import the STL/OBJ (**File → Import**) — it arrives as a `Mesh::Feature`.
2. Select the Mesh object in the tree.
3. Choose **Part → Shape from Mesh…**.
4. A dialog appears asking for the **sewing tolerance** (default 0.1 mm).
5. Click **OK**. A `Part::Feature` shell appears.
6. Proceed to [Convert to Solid](convert-to-solid.md) to close the shell.

---

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Sewing tolerance | 0.1 mm | Maximum gap between mesh triangle edges to be considered connected. Increase this value if the mesh has small gaps. |

---

## Common mistakes and pitfalls

!!! warning "Result is a shell, not a solid"
    Shape from Mesh creates a shell (open or closed surface), not a solid
    volume. You cannot use it directly in PartDesign or for Boolean operations
    that require a solid. Use [Convert to Solid](convert-to-solid.md) next.

!!! warning "Large sewing tolerance may bridge holes unintentionally"
    If the sewing tolerance is too large, gaps that should remain open will
    be bridged, creating incorrect topology. Start with the default 0.1 mm
    and increase only if necessary.

!!! warning "Non-watertight meshes cannot become solids"
    If the original mesh has holes (boundary edges), Convert to Solid will
    fail after Shape from Mesh. Repair the mesh in the Mesh workbench first
    using Mesh → Fill Holes and Mesh → Close Holes.

---

## Python API

```python
import FreeCAD as App
import Mesh, MeshPart

doc = App.newDocument()

# Load a mesh from a file
mesh_obj = doc.addObject("Mesh::Feature", "ImportedMesh")
mesh_obj.Mesh = Mesh.Mesh("/path/to/file.stl")
doc.recompute()

# Convert mesh to Part shape with 0.1 mm sewing tolerance
shape = MeshPart.meshToShape(mesh_obj.Mesh, 0.1)
feat = doc.addObject("Part::Feature", "ShapeFromMesh")
feat.Shape = shape
doc.recompute()

print(f"Shell faces: {len(feat.Shape.Faces)}")
```

---

## See also

- [Convert to Solid](convert-to-solid.md) — close the shell into a usable solid
- [Refine Shape](refine-shape.md) — merge co-planar triangles after conversion
- [Check Geometry](check-geometry.md) — verify the resulting shape
- [Points from Shape](points-from-shape.md) — extract vertices from a Part shape
- Mesh workbench → Fill Holes — repair mesh gaps before conversion
- [Part Workbench](../index.md) — workbench overview
