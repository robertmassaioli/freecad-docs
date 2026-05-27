# Mesh and Solid Conversion

> **In one sentence:** Convert between mesh objects and Part shapes, extract
> vertices from a shape, flip face normals, or close a shell into a solid.

---

## Overview

**Workbench:** Part · **Menu:** Part → Convert to Solid / Part → Reverse Shapes / …  
**Toolbar:** Part Tools · **Shortcut:** none

These four tools bridge the gap between mesh-based geometry (imported from STL,
OBJ, or created in the Mesh workbench) and OCCT B-rep shapes used by the Part
and Part Design workbenches.

| Tool | Description |
|------|-------------|
| [Shape from Mesh](#shape-from-mesh) | Wrap a mesh object as a Part shell shape |
| [Points from Shape](#points-from-shape) | Extract all vertices from a shape as a Points object |
| [Convert to Solid](#convert-to-solid) | Close a shell or surface shape into a solid |
| [Reverse Shapes](#reverse-shapes) | Flip the face normals of a shape |

---

## Shape from Mesh

**Menu:** Part → Shape from Mesh  
**What it does:** Takes a Mesh object (from the Mesh workbench or an imported
STL/OBJ) and creates a Part shape from its triangulated faces. The result is a
shell of triangular faces — not a clean B-rep solid.

### When to use it

You imported an STL and need to Boolean-cut it with a Part solid, or you need
to extract measurements from it. The mesh-as-Part-shape is not a parametric
solid, but it can participate in Boolean operations.

### Workflow: STL → solid (typical repair workflow)

1. **File → Import** the STL — it arrives in the Mesh workbench as a Mesh object.
2. **Part → Shape from Mesh** — creates a Part shell from the mesh triangles.
3. **Part → Convert to Solid** — closes the shell into a solid (if the mesh is
   watertight).
4. *(Optional)* **Part → Refine Shape** ([Copy Tools](copy-tools.md#refine-shape))
   to merge co-planar triangles into clean planar faces.
5. **Part → Check Geometry** to verify the result.

### Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Sewing tolerance | 0.1 mm | Maximum gap between mesh triangle edges to be considered connected |

### Python API

```python
import FreeCAD as App
import Mesh, Part

doc = App.newDocument()

# Import an STL as a mesh
mesh_obj = doc.addObject("Mesh::Feature", "ImportedMesh")
Mesh.open("/path/to/file.stl")   # loads into active doc

# Convert mesh to Part shape
# Select the mesh object first, then:
import MeshPart
shape = MeshPart.meshToShape(mesh_obj.Mesh, 0.1)  # tolerance 0.1 mm
feat = doc.addObject("Part::Feature", "ShapeFromMesh")
feat.Shape = shape
doc.recompute()
```

---

## Points from Shape

**Menu:** Part → Points from Shape  
**What it does:** Extracts all vertices from a Part shape and creates a Points
object (a cloud of points) containing them.

### When to use it

- You need the corner coordinates of a solid as input for another tool.
- You want to visualise the vertex positions of a complex shape.
- You are feeding vertex coordinates into a script or spreadsheet.

### Python API

```python
import FreeCAD as App

doc = App.newDocument()
box = doc.addObject("Part::Box", "Box")
box.Length = 20; box.Width = 15; box.Height = 10
doc.recompute()

# Extract vertices (use the tool interactively, or iterate manually)
vertices = box.Shape.Vertexes
for v in vertices:
    print(f"Vertex at: {v.Point}")
```

---

## Convert to Solid

**Menu:** Part → Convert to Solid  
**What it does:** Takes a Part shell or surface shape (open or closed) and
attempts to create a solid from it. Requires the input shell to be closed
(watertight — no boundary edges).

### When to use it

- After [Shape from Mesh](#shape-from-mesh) to obtain a usable solid.
- When a STEP import results in a shell rather than a solid.
- When [Shape Builder](shape-builder.md) assembled a closed shell.

### Common failures

!!! warning "Open shell — boundary edges exist"
    If the input has any boundary edges (gaps), Convert to Solid fails.
    Check in Check Geometry and close the gaps by adding missing faces.

### Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Make a closed shell from faces and convert to solid
box_shape = Part.makeBox(10, 10, 10)
shell = box_shape.Shells[0]   # already a closed shell

solid = Part.Solid(shell)
feat = doc.addObject("Part::Feature", "Solid")
feat.Shape = solid
doc.recompute()
print(f"Is solid: {solid.ShapeType}")   # Solid
```

---

## Reverse Shapes

**Menu:** Part → Reverse Shapes  
**What it does:** Creates a copy of the selected shape with all face normals
flipped. The geometry is identical; only the normal direction changes.

### When to use it

Face normals define the "outside" of a solid. Some imported shapes (particularly
from mesh conversion) have inverted normals, which causes them to appear inside-
out in rendering and to behave incorrectly in Boolean operations (a solid with
inward normals acts as empty space when cut, for example).

### When you need it

- A shape renders with a dark/black appearance (normals pointing inward).
- A Boolean operation behaves unexpectedly (fusing two shapes produces empty
  geometry or vice versa).
- Check Geometry reports inverted faces.

### Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Original shape
original = doc.addObject("Part::Box", "Original")
original.Length = 10; original.Width = 10; original.Height = 10
doc.recompute()

# Reverse normals
reversed_shape = original.Shape.reversed()
feat = doc.addObject("Part::Feature", "Reversed")
feat.Shape = reversed_shape
doc.recompute()
```

---

## See also

- [Check Geometry](check-geometry.md) — verify shapes for OCCT validity
- [Shape Builder](shape-builder.md) — assemble shells from faces manually
- [Copy Tools → Refine Shape](copy-tools.md#refine-shape) — clean up mesh-converted shapes
