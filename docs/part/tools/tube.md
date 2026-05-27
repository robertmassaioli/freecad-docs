# Tube

> **In one sentence:** Create a parametric hollow cylinder defined by inner
> radius, outer radius, and height — the typical round-bar or pipe primitive.

---

## Overview

**Workbench:** Part · **Menu:** Part → Primitives → Tube  
**Toolbar:** Part Primitives · **Shortcut:** none

The Tube primitive creates a hollow right circular cylinder. It is equivalent
to a Part Cylinder with a concentric cylindrical hole, but defined as a single
parametric object rather than a Boolean Cut.

---

## Intuition

Most mechanical hollow cylinders (pipes, bushings, sleeves, spacers) require
both an outer diameter and a bore diameter. With a plain Cylinder you would
need to subtract a second cylinder — a two-step Boolean Cut. Tube gives you
both radii in one object, keeping the tree clean.

---

## When to use it

- Pipe, bushing, or sleeve primitives.
- Round stock with a central bore.
- Any hollow cylinder where the wall thickness matters as a parameter.

## When NOT to use it

- **Solid cylinder** — use [Cylinder](cylinder.md).
- **Non-cylindrical hollow** — extrude or revolve a sketch instead.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InnerRadius | Distance | 2 mm | Radius of the bore |
| OuterRadius | Distance | 5 mm | Radius of the outer surface |
| Height | Distance | 10 mm | Axial length |
| Angle | Angle | 360° | Longitude sweep (360° = full tube) |
| Placement | Placement | Identity | Position and orientation |

> **Note:** InnerRadius must be less than OuterRadius. The wall thickness is
> OuterRadius − InnerRadius.

---

## Step-by-step walkthrough

1. **Part → Primitives → Tube.**
2. A hollow cylinder (inner R = 2, outer R = 5, H = 10 mm) appears.
3. In **Data**, set **InnerRadius** and **OuterRadius** for your pipe spec.
4. Set **Height** to the pipe length.
5. Position with **Placement**.

---

## Common mistakes and pitfalls

!!! warning "InnerRadius ≥ OuterRadius causes recompute error"
    The tube becomes invalid if the bore radius is equal to or larger than the
    outer radius. FreeCAD will report a recompute error — reduce InnerRadius.

---

## Python API

```python
import FreeCAD as App
# Tube is implemented as a Python feature in BasicShapes
import Part

doc = App.newDocument()

# The Tube is in Part.BasicShapes — use the command to create it interactively,
# or create the equivalent via a Boolean approach:

# Equivalent programmatic approach:
outer = doc.addObject("Part::Cylinder", "Outer")
outer.Radius = 15.0
outer.Height = 30.0

inner = doc.addObject("Part::Cylinder", "Inner")
inner.Radius = 10.0
inner.Height = 30.0

cut = doc.addObject("Part::Cut", "Tube")
cut.Base = outer
cut.Tool = inner

doc.recompute()
print(f"Wall thickness: {15.0 - 10.0} mm")
print(f"Volume: {cut.Shape.Volume:.2f} mm³")
```

> **Note:** The `Part::Tube` object type is also available directly in FreeCAD
> 1.0+. You can use `doc.addObject("Part::Tube", "MyTube")` and set its
> `InnerRadius`, `OuterRadius`, and `Height` properties.

---

## See also

- [Cylinder](cylinder.md) — solid cylinder
- [Torus](torus.md) — curved pipe / ring
- [Thickness](thickness.md) — hollow out an arbitrary solid to a shell
