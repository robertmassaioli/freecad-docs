# Ruled Surface

> **In one sentence:** Create a surface by linearly blending between two
> parallel edges or wires — every point on the surface lies on a straight line
> connecting corresponding points on the two rails.

---

## Overview

**Workbench:** Part · **Menu:** Part → Ruled Surface  
**Toolbar:** Part Modeling · **Shortcut:** none

A ruled surface is a special case of a surface where every generating line is
straight. Select two edges or wires (the "rails") and FreeCAD creates a surface
by linearly interpolating between them.

---

## Intuition

Imagine stretching a sheet of rubber between two parallel wires of different
shapes — the rubber takes the form of a ruled surface. The surface is not curved
in the mathematical sense (no curvature in one direction), but the two edges can
be any shape.

Classic ruled surfaces:
- **Cylinder** — two identical parallel circles.
- **Cone** — a circle and a point (degenerate case).
- **Hyperbolic paraboloid (saddle)** — two non-parallel straight lines.
- **Helicoid** — a line sweeping around an axis (if the line is the generator).

In FreeCAD, Ruled Surface is useful for:
- Connecting two different cross-sections with a smooth (but ruled) transition.
- Creating mould split-line surfaces.
- Approximating developable surfaces (sails, sheet metal).

---

## When to use it

- You need a transition surface between two rails and a straight-line
  cross-section is acceptable (no curvature).
- Sheet metal design: ruled surfaces are developable and can be unrolled flat.

## When NOT to use it

- **Smooth curved blend** — use [Loft](loft.md) which uses curve-smoothing
  algorithms.
- **Profile along a path** — use [Sweep](sweep.md).

---

## Step-by-step walkthrough

1. **Create two edges or wires** (the rails). They should be oriented in the
   same direction for a predictable result.
2. **Select the first edge/wire.**
3. **Ctrl+click the second edge/wire.**
4. **Part → Ruled Surface.**
5. A surface (shell) appears connecting the two rails.

---

## Parameters

The tool has no dialog parameters — it operates on the two pre-selected
edges/wires directly. The orientation and direction of the resulting surface
depend on the direction in which each wire is traversed.

---

## Common mistakes and pitfalls

!!! warning "Twisted surface — wire orientations are opposite"
    If the two rails are traversed in opposite directions (one clockwise, one
    counter-clockwise), the ruled surface will twist. Reverse one wire's
    direction by selecting it and using Part → Reverse Shapes, or rearrange the
    wire edges.

!!! warning "Result is a shell (surface), not a solid"
    Ruled Surface produces an open shell. To get a solid, cap the open ends
    with [Face from Wires](face-from-wires.md) and use
    [Shape Builder](shape-builder.md) or [Convert to Solid](mesh-and-solid.md#convert-to-solid).

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Two rails: a circle at Z=0 and a circle at Z=20
circle1 = Part.makeCircle(10.0, App.Vector(0, 0, 0), App.Vector(0, 0, 1))
circle2 = Part.makeCircle(15.0, App.Vector(0, 0, 20), App.Vector(0, 0, 1))

wire1 = Part.Wire([circle1])
wire2 = Part.Wire([circle2])

# Create ruled surface
surface = Part.makeRuledSurface(wire1, wire2)
feat = doc.addObject("Part::Feature", "RuledSurface")
feat.Shape = surface
doc.recompute()
```

---

## See also

- [Loft](loft.md) — smooth blend through multiple cross-sections
- [Sweep](sweep.md) — sweep a profile along a path
- [Face from Wires](face-from-wires.md) — cap the open ends of a ruled surface
