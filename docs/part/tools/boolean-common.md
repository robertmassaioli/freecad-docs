# Boolean Common (Intersection)

> **In one sentence:** Keep only the volume that is simultaneously inside both
> input shapes — the geometric intersection.

---

## Overview

**Workbench:** Part · **Menu:** Part → Boolean → Common  
**Toolbar:** Part Boolean · **Shortcut:** none

Boolean Common (intersection) produces a solid containing only the material that
exists in *both* the Base and Tool shapes. Everything not shared is discarded.

---

## Intuition

Imagine two overlapping clay lumps. Common gives you the overlap region — the
piece that would remain if you squashed the two lumps together and pulled away
everything that wasn't touching.

### Practical uses

- **Clipping a shape to a bounding volume** — Common with a box clips any
  protrusions outside the box.
- **Finding the engagement region** — how far does a plug enter a socket? Common
  shows exactly the overlapping solid.
- **Lens optics** — intersecting two spheres gives a lens shape.

---

## When to use it

- Trimming geometry to a boundary volume.
- Extracting the overlap between two imported parts to check fit.
- Creating lens or intersection-based shapes.

## When NOT to use it

- **Shapes that do not overlap** — Common produces empty geometry (a null shape).
  Verify that the shapes actually intersect before using Common.

---

## Step-by-step walkthrough

1. **Select the Base shape.**
2. **Ctrl+click the Tool shape.**
3. **Part → Boolean → Common.**
4. A `Part::Common` object appears containing only the shared volume.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base | First operand |
| Tool | Second operand |

---

## Common mistakes and pitfalls

!!! warning "Empty result — shapes do not overlap"
    If Base and Tool have no shared volume, Common produces an empty shape.
    Check Placement values and ensure the shapes visually overlap.

!!! warning "Tangent-touching shapes may produce a degenerate result"
    If the shapes touch only at a single point or edge (not overlapping in
    volume), Common may produce a degenerate edge or vertex rather than a
    solid. Ensure the shapes overlap volumetrically.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Two overlapping cylinders — intersection is a lens shape
cyl1 = doc.addObject("Part::Cylinder", "Cyl1")
cyl1.Radius = 15; cyl1.Height = 30
cyl1.Placement = App.Placement(App.Vector(-10, 0, 0), App.Rotation())

cyl2 = doc.addObject("Part::Cylinder", "Cyl2")
cyl2.Radius = 15; cyl2.Height = 30
cyl2.Placement = App.Placement(App.Vector(10, 0, 0), App.Rotation())
doc.recompute()

common = doc.addObject("Part::Common", "Intersection")
common.Base = cyl1
common.Tool = cyl2
doc.recompute()

print(f"Intersection volume: {common.Shape.Volume:.2f} mm³")
```

---

## See also

- [Boolean Cut](boolean-cut.md) — remove one shape from another
- [Boolean Fuse](boolean-fuse.md) — union of both shapes
- [Boolean Dialog](boolean.md) — choose operation interactively
- [Boolean XOR](boolean-xor.md) — keep only the non-shared regions
