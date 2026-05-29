# Boolean XOR

> **In one sentence:** Keep only the material that is in one shape but not
> both — the symmetric difference — discarding the shared overlapping region.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Split → Boolean XOR  
**Shortcut:** none

Computes the symmetric difference of two shapes: keeps only the material that
belongs exclusively to one shape or the other, and removes the region where
they overlap. XOR = Fuse − Common.

---

## Intuition

Think of a Venn diagram: Boolean Fuse is both circles; Boolean Common is only
the overlap; Boolean XOR is both circles *with the overlap cut out* — just the
unique parts of each.

Use XOR to isolate the engagement region between two mating parts, or to
visualise the material that would be removed or added to make one design
match another.

---

## When to use it

- Cutting out the engagement region between two mating parts to see what
  interferences exist.
- Visualising the non-overlapping material when checking part fits for assembly.
- Generating the "difference region" for tolerance analysis or design comparison.
- Removing the common material from two overlapping decorative elements.

## When NOT to use it

- **Don't use it when shapes don't overlap** — XOR of non-overlapping shapes
  is equivalent to Fuse, which is rarely what you want from XOR semantics.
- **Don't use it when you want to keep the overlap** — use
  [Boolean Common](boolean-common.md) to extract only the shared region.

---

## Step-by-step

1. Select the first shape.
2. **Ctrl+click** the second shape.
3. Choose **Part → Split → Boolean XOR**.
4. A compound appears with the two non-overlapping fragments.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | The two shapes to compute XOR on |

---

## Common mistakes and pitfalls

!!! warning "Non-overlapping shapes produce a Fuse equivalent"
    When two shapes do not share any volume, XOR returns both shapes unchanged
    (same result as Fuse). This is mathematically correct but rarely useful.
    Verify the shapes actually overlap before using this tool.

!!! warning "Result is a compound of two fragments"
    The output is a compound containing the two exclusive regions. Use
    **Part → Explode Compound** to work with them independently.

!!! warning "XOR of identical shapes is empty"
    If both shapes are identical, the XOR result has zero volume (all material
    is in the "common" region that gets removed). This is correct behaviour.

---

## Python API

```python
import FreeCAD as App
import BOPTools.SplitFeatures as SF

doc = App.newDocument()

s1 = doc.addObject("Part::Box", "Shape1")
s1.Length = 20; s1.Width = 20; s1.Height = 20

s2 = doc.addObject("Part::Box", "Shape2")
s2.Length = 20; s2.Width = 20; s2.Height = 20
s2.Placement = App.Placement(App.Vector(10, 0, 0), App.Rotation())
doc.recompute()

xor = SF.makeXOR(name="XOR")
xor.Objects = [s1, s2]
xor.Proxy.execute(xor)
doc.recompute()

print(f"XOR result solids: {len(xor.Shape.Solids)}")   # 2
```

---

## See also

- [Boolean Common](boolean-common.md) — keep only the shared (overlapping) region
- [Boolean Fuse](boolean-fuse.md) — union of both shapes
- [Boolean Fragments](boolean-fragments.md) — split all inputs at all mutual intersections
- [Slice to Compound](slice-to-compound.md) — non-destructive slicing
- [Part Workbench](../index.md) — workbench overview
