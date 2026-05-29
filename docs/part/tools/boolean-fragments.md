# Boolean Fragments

> **In one sentence:** Split all input shapes at every mutual intersection,
> producing a compound of every non-overlapping fragment with no material
> added or removed.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Split → Boolean Fragments  
**Shortcut:** none

Takes any number of shapes, computes all pairwise intersections, and splits
every shape along every intersection boundary. The result is a compound of all
non-overlapping fragments. Total volume is preserved — the inputs are partitioned,
not merged or subtracted.

---

## Intuition

Standard Boolean Cut/Fuse/Common always produce one result shape, discarding
the removed pieces. Boolean Fragments is different: **every fragment is
preserved**. Nothing is thrown away.

Imagine three overlapping spheres. Boolean Fragments divides them into seven
distinct regions: the three unique parts of each sphere, the three pairwise
overlaps, and the triple-overlap centre. Each region becomes a separate solid
in the output compound.

This makes Boolean Fragments essential for:

- **FEA/CFD meshing** — adjacent parts need conforming meshes (shared nodes at
  boundaries). Fragmenting all parts first ensures the mesh generator sees
  shared boundary faces.
- **Spatial analysis** — understanding exactly which regions two solids overlap.
- **Voronoi/split patterns** — dividing a volume into prescribed sub-volumes.

---

## When to use it

- Preparing a multi-body assembly for FEA meshing (conforming mesh boundaries).
- Analysing how multiple components overlap in an assembly.
- Cutting a body with multiple planes simultaneously.
- Generating all fragments of a complex overlap without discarding any piece.

## When NOT to use it

- **Don't use it if you only need one result shape** — use
  [Boolean Cut](boolean-cut.md), [Boolean Fuse](boolean-fuse.md), or
  [Boolean Common](boolean-common.md) instead.
- **Don't use it on non-intersecting shapes** — the result will be a compound
  of the original unchanged shapes, which is rarely useful.

---

## Step-by-step

1. Select all the shapes to fragment (any number; hold Ctrl to multi-select).
2. Choose **Part → Split → Boolean Fragments**.
3. A compound appears containing all fragments.
4. Use **Part → Explode Compound** to separate them into individual objects
   if needed.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | All input shapes to fragment |
| Mode | Standard / Split / CompSolid — controls how touching fragments are grouped in the output compound |

---

## Common mistakes and pitfalls

!!! warning "Non-intersecting shapes produce an unchanged compound"
    If none of the inputs overlap, Boolean Fragments returns a compound of the
    original shapes. Verify that inputs actually intersect before using this
    tool.

!!! warning "Large numbers of inputs can be slow"
    Each pairwise intersection is computed. With N shapes, there are N(N−1)/2
    intersection checks. Keep input counts reasonable (< 20) for interactive
    use.

!!! warning "Mode affects downstream usability"
    In CompSolid mode, touching fragments are grouped as a compound solid —
    useful for FEA but may confuse tools that expect independent solids. Use
    Standard mode for general-purpose use.

---

## Python API

```python
import FreeCAD as App
import BOPTools.SplitFeatures as SF

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 30; box.Height = 30

cyl = doc.addObject("Part::Cylinder", "Cyl")
cyl.Radius = 10; cyl.Height = 40
cyl.Placement = App.Placement(App.Vector(15, 15, -5), App.Rotation())
doc.recompute()

frags = SF.makeBooleanFragments(name="Fragments")
frags.Objects = [box, cyl]
frags.Proxy.execute(frags)
doc.recompute()

print(f"Number of fragments: {len(frags.Shape.Solids)}")
```

---

## See also

- [Slice Apart](slice-apart.md) — cut with a tool, producing separate objects
- [Slice to Compound](slice-to-compound.md) — cut with a tool, keeping results in a compound
- [Boolean XOR](boolean-xor.md) — keep only non-overlapping portions of two shapes
- [Boolean Cut](boolean-cut.md) — simple two-shape subtraction
- [Part Workbench](../index.md) — workbench overview
