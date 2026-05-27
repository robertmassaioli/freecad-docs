# Sketcher Workbench

The Sketcher workbench is the foundation of parametric solid modelling in FreeCAD.
You use it to draw precise 2-D geometry that is then handed off to Part Design
(or Part) to become 3-D solids.

---

## What Sketcher is for

A *sketch* is a fully-defined 2-D drawing attached to a plane. Each piece of
geometry in the sketch — lines, arcs, circles, splines — can be locked to an exact
size and position by applying *constraints*. When every degree of freedom has been
removed (the sketch is *fully constrained*) the geometry turns green and the sketch
is ready to drive a solid feature.

The constraint-based approach is what makes FreeCAD models *parametric*: change a
dimension value and the entire sketch — and every feature that depends on it —
updates automatically.

---

## The Sketcher workflow

1. **Open a sketch** on a plane (XY, XZ, YZ, a datum plane, or a face).
2. **Draw geometry** — lines, arcs, rectangles, splines, etc.
3. **Apply constraints** — geometric (parallel, tangent, …) then dimensional
   (distance, radius, angle, …).
4. **Achieve full constraint** — all elements turn green; the constraint counter
   reads "Fully constrained".
5. **Close the sketch** and use it as input to Pad, Pocket, Revolution, etc.

---

## Constraint colours

| Colour | Meaning |
|--------|---------|
| White / default | Unconstrained (still has degrees of freedom) |
| Green | Fully constrained — no remaining degrees of freedom |
| Red | Over-constrained or conflicting constraints — resolve before closing |
| Blue | Reference (construction) constraint or dimension — does not constrain geometry |
| Yellow | Partially redundant constraint |

---

## Key concepts

### Degrees of freedom (DoF)

Every 2-D point has 2 DoF (it can slide in X and Y). Every line between two
points has 4 DoF, and so on. A constraint removes one or more DoF. The Solver
panel in the left sidebar counts the remaining DoF in real time: the goal is
always zero.

### Driving vs reference constraints

A *driving* constraint commands geometry to a specific value. A *reference*
constraint reads the current value without constraining anything — it is a
measurement label, not a command. See the individual constraint pages for details.

### Construction geometry

Any geometry element can be toggled to *construction mode* (thin dashed line).
Construction geometry is used as a scaffolding aid — it participates in constraints
but is not included in the profile passed to Part Design. This lets you add helper
circles, centre lines, and guide lines without polluting the final shape.

---

## Tools overview

| Group | Tools |
|-------|-------|
| [Sketch Lifecycle](tools/index.md#sketch-lifecycle) | New Sketch, Edit Sketch, Leave Sketch, Map Sketch, Reorient, Validate, Mirror, Merge |
| [Geometry](tools/index.md#geometry) | Point, Line, Polyline, Arcs, Circles, Conics, Rectangle, Polygon, Slot, Fillet, Chamfer, B-Spline |
| [Editing](tools/index.md#editing) | Trim, Split, Extend, External Geometry, Carbon Copy |
| [Geometric Constraints](tools/index.md#geometric-constraints) | Coincident, Horizontal/Vertical, Parallel, Perpendicular, Tangent, Equal, Symmetric, Block, Point on Object, Lock |
| [Dimensional Constraints](tools/index.md#dimensional-constraints) | Dimension, Distance, Radius/Diameter, Angle, Snell's Law, Toggle Driving |
| [Transforms](tools/index.md#transform-tools) | Offset, Translate, Rotate, Scale, Mirror, Copy/Clone/Move |
| [Analysis](tools/index.md#analysis-tools) | Select tools, Delete all, Remove axes alignment, Toggle internal geometry |

See the [Tools index](tools/index.md) for a complete alphabetical reference.

---

## Relationship to Part Design

Sketcher does not build solids — it produces a 2-D profile. Part Design
takes that profile and extrudes, revolves, sweeps, or lofts it into a solid.
Every Pad, Pocket, Revolution, and similar feature in Part Design requires a
Sketcher sketch as its starting point.

If you are new to FreeCAD, the typical learning path is:

1. Learn the Sketcher (this workbench) — how to draw and fully constrain a sketch.
2. Learn [Part Design](../part-design/index.md) — how to turn sketches into solids.
