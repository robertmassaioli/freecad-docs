# Copy Tools

> **In one sentence:** Make static or transformed copies of shapes, copy
> individual sub-elements, or clean up redundant geometry from a shape.

---

## Overview

**Workbench:** Part · **Menu:** Part → Create a Copy → …  
**Toolbar:** Part Tools · **Shortcut:** none

These four tools all create new Part shapes derived from existing ones, without
modifying the original.

| Tool | Description |
|------|-------------|
| [Simple Copy](#simple-copy) | Create a static, non-parametric copy |
| [Transformed Copy](#transformed-copy) | Copy with an applied placement transform |
| [Shape Element Copy](#shape-element-copy) | Copy a single face, edge, or vertex from a shape |
| [Refine Shape](#refine-shape) | Remove redundant edges and merge co-planar faces |

---

## Intuition

Every tool in this group creates a new Part shape derived from an existing one,
leaving the original unchanged. The key question is *how much* of the
parametric history to preserve:

- **Simple Copy** breaks the link entirely — a frozen snapshot that never
  changes again, useful as a stable base when you need to branch the design
  or lock in a state for export.
- **Transformed Copy** bakes the current Placement into the geometry so the
  shape "owns" its position in world space rather than relying on a matrix.
- **Shape Element Copy** extracts a single topological element (one face, one
  edge, one vertex) as its own standalone feature — useful when you need just
  a face to project onto, or an edge to use as a sweep spine.
- **Refine Shape** is the odd one out: it does not copy the shape for
  versioning purposes but cleans up its topology — merging co-planar faces
  and removing seam edges left over from Boolean operations. Run it before
  Fillet or Chamfer to avoid failures on spurious edges.

---

## Simple Copy

**Menu:** Part → Create a Copy → Simple Copy  
**What it does:** Creates a static copy of the selected shape. The copy is
frozen — it does not update if the original changes. This is useful for
breaking the parametric dependency when you need a stable reference geometry.

### When to use it

- You want to lock the current state of a shape before further modifications.
- You need to pass a shape to an external tool that cannot handle live Part
  features.
- You want two independent variants starting from the same base shape.

### Difference from Shape Element Copy

Simple Copy copies the entire shape. Shape Element Copy copies one sub-element
(one face, one edge, one vertex).

### Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

original = doc.addObject("Part::Box", "Original")
original.Length = 20; original.Width = 15; original.Height = 10
doc.recompute()

# Create a static copy
copy_shape = original.Shape.copy()
copy_feat = doc.addObject("Part::Feature", "StaticCopy")
copy_feat.Shape = copy_shape
doc.recompute()

# Now modify the original — the copy is unaffected
original.Length = 40
doc.recompute()
print(f"Copy length: {copy_feat.Shape.BoundBox.XLength}")   # still 20
```

---

## Transformed Copy

**Menu:** Part → Create a Copy → Transformed Copy  
**What it does:** Creates a static copy of the selected shape with the shape's
current placement baked into the geometry. The result is a shape whose vertices
have been transformed to world coordinates, and whose Placement is reset to
identity.

### When to use it

- You placed a shape at an angle using Placement, and now want to export or
  Boolean-combine it without relying on the Placement matrix.
- You want a "flattened" version of a shape with placement baked in.

### Python API

```python
import FreeCAD as App

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Placement = App.Placement(
    App.Vector(10, 20, 5),
    App.Rotation(App.Vector(0, 0, 1), 45)  # 45° rotation
)
doc.recompute()

# Bake the placement into the shape
baked = box.Shape.copy()
baked.Placement = App.Placement()  # reset to identity
feat = doc.addObject("Part::Feature", "TransformedCopy")
feat.Shape = baked
doc.recompute()
```

---

## Shape Element Copy

**Menu:** Part → Create a Copy → Shape Element Copy  
**What it does:** Copies a single selected sub-element (one face, one edge, or
one vertex) from a shape into a new standalone Part feature.

### When to use it

- You need a particular face as a standalone surface (e.g., to use as a
  datum plane reference or as a Projection on Surface target).
- You need a specific edge as a sweep spine.
- You want to inspect or measure a single face without the rest of the solid.

### Step-by-step

1. Select the parent solid in the tree.
2. In the viewport, **Ctrl+click** (or single-click) the face/edge/vertex you want.
3. **Part → Create a Copy → Shape Element Copy.**
4. A new Part Feature containing only that sub-element appears.

### Python API

```python
import FreeCAD as App

doc = App.newDocument()
box = doc.addObject("Part::Box", "Box")
doc.recompute()

# Extract the top face (index 5 for the top of a default box)
top_face = box.Shape.Faces[5]
feat = doc.addObject("Part::Feature", "TopFace")
feat.Shape = top_face
doc.recompute()
```

---

## Refine Shape

**Menu:** Part → Create a Copy → Refine Shape  
**What it does:** Creates a refined copy of a shape by:
- Removing redundant internal edges where adjacent faces are co-planar.
- Merging tangentially adjacent faces.
- Cleaning up B-rep topology left over from Boolean operations.

### When to use it

After a Boolean Cut or Fuse, FreeCAD preserves all the original face
boundaries on the result — even where two faces are now perfectly co-planar and
should be one face. Refine Shape merges them, producing cleaner geometry with
fewer faces. This is particularly important when the result will be filletted or
chamfered (excess edges on a planar region can cause fillet failures).

### When NOT to use it

- On shapes with intentionally coincident edges (e.g., a face with a seam that
  must remain as two separate faces for downstream selection).
- If you need to reference specific faces by index — after refining, face
  indices may change.

### Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a Boolean operation that produces redundant edges
box1 = doc.addObject("Part::Box", "Box1")
box1.Length = 20; box1.Width = 10; box1.Height = 10

box2 = doc.addObject("Part::Box", "Box2")
box2.Length = 20; box2.Width = 10; box2.Height = 10
box2.Placement = App.Placement(App.Vector(20, 0, 0), App.Rotation())

fuse = doc.addObject("Part::Fuse", "Fused")
fuse.Base = box1
fuse.Tool = box2
doc.recompute()

print(f"Faces before refine: {len(fuse.Shape.Faces)}")   # many redundant faces

# Refine the shape
refined_shape = fuse.Shape.removeSplitter()
feat = doc.addObject("Part::Feature", "Refined")
feat.Shape = refined_shape
doc.recompute()
print(f"Faces after refine: {len(feat.Shape.Faces)}")   # fewer, cleaner faces
```

---

## See also

- [Boolean Fuse](boolean-fuse.md) — merge shapes (produces redundant edges; refine afterwards)
- [Boolean Cut](boolean-cut.md) — subtract shapes (same advice)
- [Check Geometry](check-geometry.md) — verify shape validity after refinement
- [Shape Builder](shape-builder.md) — manual topology assembly
