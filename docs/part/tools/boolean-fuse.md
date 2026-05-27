# Boolean Fuse (Union)

> **In one sentence:** Merge two shapes into a single solid by taking their
> union — the resulting solid contains all material from both inputs.

---

## Overview

**Workbench:** Part · **Menu:** Part → Boolean → Fuse  
**Toolbar:** Part Boolean · **Shortcut:** none

Boolean Fuse (also called "union" in set theory) produces a solid that
encompasses all the volume of both operands. Internal coincident faces where the
two shapes were touching are removed, leaving a clean watertight solid.

---

## Intuition

Think of gluing two metal bars together: the result is one continuous piece with
no internal boundary. The Fuse operation is mathematically equivalent — it takes
the union of the two volumes and produces a single manifold solid.

Common uses:

- Adding a boss to a base plate (fuse a cylinder onto a box).
- Building an L-bracket by fusing two rectangular blocks.
- Merging several bodies that came in as separate STEP parts.

---

## When to use it

- Combining two touching or overlapping solids into one.
- Building complex geometry from simple primitives (constructive solid geometry).
- Unifying separate imported STEP bodies that should be a single piece.

## When NOT to use it

- **Shapes that do not touch or overlap** — Fuse succeeds but produces a
  compound with two separate shells, not one solid. Use
  [Make Compound](compound.md#make-compound) to group non-touching shapes.
- **Inside a Part Design Body** — use Body Boolean (Fuse) or simply place
  additive features next to each other.
- **More than two shapes** — apply successive Fuses, or use
  [Boolean Fragments](split-features.md#boolean-fragments).

---

## Step-by-step walkthrough

1. **Select the first shape** (Base).
2. **Ctrl+click the second shape** (Tool).
3. **Part → Boolean → Fuse.**
4. A `Part::Fuse` object appears.
5. Optionally run [Refine Shape](copy-tools.md#refine-shape) to merge redundant
   co-planar faces.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base | First operand |
| Tool | Second operand |
| Refine | Remove redundant edges from the result (toggle in Data panel) |

---

## Common mistakes and pitfalls

!!! warning "Non-touching shapes produce a compound, not a solid"
    If Base and Tool do not share any volume, Fuse returns a compound of two
    separate solids. Move one shape to overlap the other before fusing.

!!! warning "Redundant faces after fusing co-planar shapes"
    When you fuse two boxes that share a flat face, the shared face becomes an
    internal seam in the result. Use **Refine Shape** to remove it. This is
    important before running Fillet or Chamfer.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Two overlapping boxes: form an L-shape
box1 = doc.addObject("Part::Box", "Horizontal")
box1.Length = 60; box1.Width = 10; box1.Height = 10

box2 = doc.addObject("Part::Box", "Vertical")
box2.Length = 10; box2.Width = 10; box2.Height = 50
# They share the first 10×10×10 corner
doc.recompute()

fuse = doc.addObject("Part::Fuse", "LBracket")
fuse.Base = box1
fuse.Tool = box2
doc.recompute()

# Refine to remove the internal seam
import Part
refined = fuse.Shape.removeSplitter()
feat = doc.addObject("Part::Feature", "LBracketRefined")
feat.Shape = refined
doc.recompute()

print(f"Volume: {feat.Shape.Volume:.2f} mm³")
```

---

## See also

- [Boolean Cut](boolean-cut.md) — subtract one shape from another
- [Boolean Common](boolean-common.md) — keep only the shared volume
- [Refine Shape](copy-tools.md#refine-shape) — clean up co-planar seams after fusing
- [Boolean Fragments](split-features.md#boolean-fragments) — advanced multi-shape Boolean
