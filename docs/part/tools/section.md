# Section

> **In one sentence:** Find the cross-section wire where two shapes intersect
> — the 2-D curve that lies exactly on both surfaces simultaneously.

---

## Overview

**Workbench:** Part · **Menu:** Part → Section  
**Toolbar:** Part Boolean · **Shortcut:** none

Part Section computes the **intersection wire** between two shapes. Where
[Boolean Common](boolean-common.md) returns the overlapping *volume*, Section
returns only the *curve* (edge or set of edges) where the two surfaces meet.
The result is a wire or edge, not a solid.

---

## Intuition

If you press a flat plane into a sphere, the contact curve is a circle. That
circle is the Section of the sphere and the plane. Section is a pure 2-D
intersection result — it gives you the outline rather than the volume.

Common uses:
- **Generating a cross-section profile** for further use as a sweep or loft
  section.
- **Finding the intersection curve** between an imported surface and a plane,
  to extract a 2-D outline.
- **Design verification** — check where two components touch.

---

## When to use it

- You need the intersection curve between two shapes (not their shared volume).
- You want to extract a planar cross-section wire from a solid.
- You need the contact boundary between a mating surface and its seat.

## When NOT to use it

- **You want the shared volume** — use [Boolean Common](boolean-common.md).
- **You want multiple parallel cross-sections** — use
  [Cross-Sections](cross-sections.md).

---

## Step-by-step walkthrough

1. **Select the first shape** (the shape to section).
2. **Ctrl+click the second shape** (the cutting/sectioning shape — often a flat
   plane or a datum plane).
3. **Part → Section.**
4. A `Part::Section` wire object appears.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base | First shape |
| Tool | Second shape (sectioning plane or solid) |

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Sphere to section
sphere = doc.addObject("Part::Sphere", "Sphere")
sphere.Radius = 20.0

# Cutting plane (thin box as plane substitute)
plane = doc.addObject("Part::Box", "Plane")
plane.Length = 60; plane.Width = 60; plane.Height = 0.001
plane.Placement = App.Placement(App.Vector(-30, -30, 10), App.Rotation())
doc.recompute()

# Section
section = doc.addObject("Part::Section", "SectionWire")
section.Base = sphere
section.Tool = plane
doc.recompute()

# The result is a circle wire at Z=10
print(f"Section shape type: {section.Shape.ShapeType}")   # Wire
print(f"Length: {section.Shape.Length:.2f} mm")           # circumference
```

---

## See also

- [Cross-Sections](cross-sections.md) — multiple parallel cross-section wires
- [Boolean Common](boolean-common.md) — shared volume instead of wire
- [Persistent Section Cut](section-cut.md) — live clipping plane for the viewport
