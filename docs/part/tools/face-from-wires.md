# Face from Wires

> **In one sentence:** Create a flat planar face from one or more closed wire
> profiles — the outer wire becomes the boundary and additional wires become
> holes.

---

## Overview

**Workbench:** Part · **Menu:** Part → Make Face from Wires  
**Toolbar:** Part Modeling · **Shortcut:** none

Face from Wires takes one or more coplanar closed wires and constructs a planar
face from them. The first (outermost) wire defines the face boundary; additional
wires inside it define holes. The resulting face can then be used for
[Extrude](extrude.md), [Loft](loft.md), or [Sweep](sweep.md).

---

## Intuition

A wire is just a chain of edges — it has no area. To extrude, revolve, or loft
a 2-D profile, you need a face. Face from Wires bridges the gap: it fills in
the region bounded by the wire.

With multiple wires:
- The **outer wire** becomes the face boundary.
- Each **inner wire** punches a hole in the face.

This lets you create complex profiles (a rectangular face with a round hole, for
example) from simple wire primitives.

---

## When to use it

- You have a [Primitives dialog](primitives.md) circle or polygon and want to
  extrude it into a solid.
- You constructed a custom wire from edges and want to turn it into a face.
- You need a face with holes from separately constructed wires.

## When NOT to use it

- **Sketcher sketch** — a Sketcher sketch is already a face when closed; use
  [Extrude](extrude.md) directly.
- **Non-planar wires** — Face from Wires requires all wires to be coplanar.
  Use [Loft](loft.md) or [Sweep](sweep.md) for non-planar profiles.

---

## Step-by-step walkthrough

### Simple face from one wire

1. Create a closed wire (e.g., from [Shape Builder](shape-builder.md) or
   [Primitives](primitives.md)).
2. **Select the wire.**
3. **Part → Make Face from Wires.**
4. A planar face appears.
5. Use [Extrude](extrude.md) on the face to get a solid.

### Face with a hole (outer wire + inner wire)

1. Create the outer closed wire (e.g., a rectangle wire).
2. Create the inner closed wire (e.g., a circle wire positioned inside the
   rectangle).
3. **Ctrl+select both wires** (outer first, then inner).
4. **Part → Make Face from Wires.**
5. A face with a hole appears — the rectangle with the circle cut out.

---

## Common mistakes and pitfalls

!!! warning "Wires must be closed and coplanar"
    Any open wire or wire not in the same plane as the others causes an error.
    Ensure all edges in each wire share endpoints and all wires lie on the same
    plane.

!!! warning "Inner wire must be inside the outer wire"
    If the inner wire is outside the boundary of the outer wire, the result is
    invalid or undefined. Check the geometry before running the tool.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Outer rectangle wire
p1 = App.Vector(0, 0, 0)
p2 = App.Vector(30, 0, 0)
p3 = App.Vector(30, 20, 0)
p4 = App.Vector(0, 20, 0)
outer = Part.Wire([
    Part.makeLine(p1, p2),
    Part.makeLine(p2, p3),
    Part.makeLine(p3, p4),
    Part.makeLine(p4, p1),
])

# Inner circle wire (hole at centre)
circle_edge = Part.makeCircle(5.0, App.Vector(15, 10, 0), App.Vector(0, 0, 1))
inner = Part.Wire([circle_edge])

# Make face with hole
face = Part.Face([outer, inner])   # outer first, then holes
feat = doc.addObject("Part::Feature", "FaceWithHole")
feat.Shape = face
doc.recompute()

# Extrude to 10 mm solid
solid = face.extrude(App.Vector(0, 0, 10))
ext_feat = doc.addObject("Part::Feature", "ExtrudedPlate")
ext_feat.Shape = solid
doc.recompute()
print(f"Volume: {ext_feat.Shape.Volume:.2f} mm³")
```

---

## See also

- [Extrude](extrude.md) — turn the face into a solid
- [Sweep](sweep.md) — sweep the face along a path
- [Shape Builder](shape-builder.md) — build wires from edges
- [Primitives](primitives.md) — create circle/polygon wires
