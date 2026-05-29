# Shape Builder

> **In one sentence:** Compose new Part shapes by combining existing vertices,
> edges, wires, faces, and shells — building up from lower-level topological
> elements to higher-level ones.

---

## Overview

**Workbench:** Part · **Menu:** Part → Shape Builder  
**Toolbar:** Part Tools · **Shortcut:** none

Shape Builder opens a dialog that lets you create higher-level OCCT topology
from lower-level elements. You can build an edge from two vertices, a wire from
edges, a face from wires, a shell from faces, or a solid from shells — one step
up the B-rep topology ladder at a time.

---

## Intuition

FreeCAD's B-rep (boundary representation) topology has a strict hierarchy:

```
Vertex → Edge → Wire → Face → Shell → Solid → Compound
```

Sometimes imported geometry arrives with gaps or missing topology: you have
faces but no shell, or edges but no wire. Shape Builder lets you explicitly
assemble the missing higher-level topology from the lower-level pieces you
already have.

This is a power-user tool. For routine modelling, use [Extrude](extrude.md),
[Loft](loft.md), or [Face from Wires](face-from-wires.md) instead.

---

## When to use it

- Repairing imported STEP/IGES that has disconnected faces: select all faces
  and build a Shell, then a Solid.
- Creating custom wires from individual edges for use as sweep spines.
- Building compound shapes from manually positioned elements.

## When NOT to use it

- **Routine face creation** — use [Face from Wires](face-from-wires.md).
- **Merging two solids** — use [Boolean Fuse](boolean-fuse.md).

---

## Available constructions

| Mode | Input | Output | Use case |
|------|-------|--------|----------|
| Edge from two vertices | 2 vertices | 1 edge | Connect two points with a straight line |
| Wire from edges | N edges (connected) | 1 wire | Assemble a closed or open path from individual edges |
| Face from wires | 1+ closed wires | 1 face | Turn wire outlines into a planar face |
| Shell from faces | N faces | 1 shell | Group faces into an open or closed shell |
| Solid from shell | 1 closed shell | 1 solid | Close a shell into a watertight solid |
| Compound from shapes | N shapes (any type) | 1 compound | Group arbitrary shapes without merging |

---

## Step-by-step walkthrough

### Building a solid from imported faces

1. Import a STEP file that arrives as disconnected faces.
2. **Part → Shape Builder.**
3. In the dialog, select **Shell from Faces**.
4. Click all the faces in the viewport (or use Ctrl+A if all are desired).
5. Click **Create** — a shell object appears.
6. Change the mode to **Solid from Shell**.
7. Select the newly created shell.
8. Click **Create** — a solid appears.
9. Use [Check Geometry](check-geometry.md) to verify it is valid.

### Building a wire from edges

1. Create or import several edges that share endpoints.
2. **Part → Shape Builder.**
3. Select **Wire from Edges**.
4. Click each edge in order.
5. Click **Create** — a wire object appears.
6. Use the wire as a sweep spine with [Sweep](sweep.md).

---

## Common mistakes and pitfalls

!!! warning "Shell from Faces requires no gaps"
    The shell construction succeeds only when all selected faces share edges
    without gaps. Even a small gap causes OCCT to reject the shell. Use
    [Check Geometry](check-geometry.md) to detect gaps.

!!! warning "Solid from Shell requires a closed shell"
    An open shell (with boundary edges) cannot become a solid. Close all
    openings with additional faces before converting to solid.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Build a wire from two edges
v1 = Part.Vertex(0, 0, 0)
v2 = Part.Vertex(10, 0, 0)
v3 = Part.Vertex(10, 10, 0)
v4 = Part.Vertex(0, 10, 0)

e1 = Part.makeLine(v1.Point, v2.Point)
e2 = Part.makeLine(v2.Point, v3.Point)
e3 = Part.makeLine(v3.Point, v4.Point)
e4 = Part.makeLine(v4.Point, v1.Point)

wire = Part.Wire([e1, e2, e3, e4])
face = Part.Face(wire)
shell = Part.Shell([face])

# Add top face and bottom face to close the shell...
# (simplified: just show the wire/face approach)
Part.show(face)
doc.recompute()

# Extrude the face to get a solid
solid = face.extrude(App.Vector(0, 0, 5))
feature = doc.addObject("Part::Feature", "ExtrudedBox")
feature.Shape = solid
doc.recompute()
```

---

## See also

- [Face from Wires](face-from-wires.md) — simpler dedicated tool for face creation
- [Check Geometry](check-geometry.md) — verify shells and solids for validity
- [Extrude](extrude.md) — build a solid from a face or wire
- [Convert to Solid](convert-to-solid.md) — convert a shell to solid
