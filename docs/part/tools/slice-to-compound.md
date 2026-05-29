# Slice to Compound

> **In one sentence:** Cut a Base shape using a Tool shape and keep all
> resulting fragments in a single compound object, leaving both originals
> intact.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Split → Slice to Compound  
**Shortcut:** none

Cuts a Base shape using a Tool shape and bundles all resulting fragments into a
single compound `Part::Feature`. The operation is non-destructive — the original
Base and Tool remain as separate objects in the model tree.

---

## Intuition

Like [Slice Apart](slice-apart.md), but the sliced pieces stay bundled together
in one container. The original shapes are still there, and the compound holds
the result.

Use Slice to Compound when you want to keep the fragments linked together for
further operations — export, assembly, or FEA — without separating them into
independent tree objects.

---

## When to use it

- Slicing geometry for export: you want one file containing all slices as
  a compound.
- Slicing a solid along a datum plane to apply boundary conditions in FEA.
- Analysing a cross-section: slice the part and inspect the resulting faces.
- Non-destructive splitting where you may want to undo or adjust the cut.

## When NOT to use it

- **Don't use it if you need independent objects after slicing** — use
  [Slice Apart](slice-apart.md) to get separate tree entries per fragment.
- **Don't use it as a replacement for Boolean Cut** — if you want to remove one
  piece and keep the other, use [Boolean Cut](boolean-cut.md) instead.

---

## Step-by-step

1. Select the **Base shape** (the shape to cut).
2. **Ctrl+click** the **Tool shape** (the cutting surface or solid).
3. Choose **Part → Split → Slice to Compound**.
4. A compound containing all fragments appears in the tree. The Base and Tool
   remain unchanged.
5. Use **Part → Explode Compound** to separate fragments into individual objects
   later if needed.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base | The shape to cut (kept in tree; result compound is separate) |
| Tool | The cutting shape (kept in tree; defines the cut boundary) |

---

## Common mistakes and pitfalls

!!! warning "Tool must intersect the Base"
    If the Tool does not cross the Base, no slice is performed and the compound
    contains only the unmodified Base.

!!! warning "Result is a compound — explode to access individual fragments"
    The output is a single compound object. To work with individual fragments
    separately, use **Part → Explode Compound** or **Part → Convert → Explode
    Compound**.

!!! warning "Faces at the cut boundary are shared"
    The cut boundary faces appear in both fragments of the compound. This is
    intentional for FEA conforming meshes but may be unexpected in other
    contexts.

---

## Python API

```python
import FreeCAD as App
import BOPTools.SplitFeatures as SF

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 40; box.Width = 20; box.Height = 20

# Cutting plane as a thin slab
plane = doc.addObject("Part::Box", "CutPlane")
plane.Length = 50; plane.Width = 50; plane.Height = 0.001
plane.Placement = App.Placement(App.Vector(-5, -15, 10), App.Rotation())
doc.recompute()

sliced = SF.makeSlice(name="SlicedCompound")
sliced.Objects = [box, plane]
sliced.Proxy.execute(sliced)
doc.recompute()

print(f"Fragments in compound: {len(sliced.Shape.Solids)}")
```

---

## See also

- [Slice Apart](slice-apart.md) — destructive version that creates separate objects per fragment
- [Boolean Fragments](boolean-fragments.md) — split multiple shapes at all mutual intersections
- [Boolean XOR](boolean-xor.md) — keep only non-overlapping portions
- [Boolean Cut](boolean-cut.md) — simple two-shape subtraction
- [Part Workbench](../index.md) — workbench overview
