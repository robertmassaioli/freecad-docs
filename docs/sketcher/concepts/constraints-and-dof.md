# Constraints and Degrees of Freedom

> **In one sentence:** Every piece of Sketcher geometry starts with a
> fixed number of degrees of freedom (DoF); each constraint removes one
> or more of those DoF until the sketch reaches zero — at which point it
> is *fully constrained* and ready to drive a solid feature.

---

## What is a Degree of Freedom?

A **degree of freedom** (DoF) is one independent way a piece of geometry
can change without violating any rule. In a 2-D sketch every unconstrained
point can slide in X *and* in Y — that is two DoF per point.

| Geometry type | DoF when unconstrained | Why |
|---------------|----------------------|-----|
| Point | 2 | Can move in X and Y |
| Line segment | 4 | Two endpoints × 2 DoF each |
| Circle | 3 | Centre (2 DoF) + radius (1 DoF) |
| Arc | 5 | Centre (2) + radius (1) + two angles (2) |
| B-spline | varies | Each control point contributes 2 DoF |

The Sketcher Solver panel (left sidebar, while a sketch is open) shows the
running DoF count. The target is always **0**.

---

## Constraint colours

FreeCAD uses geometry colours to communicate the solver state at a glance:

| Colour | Meaning |
|--------|---------|
| White / default | Unconstrained — one or more DoF remain |
| Green | Fully constrained — zero DoF |
| Red | Over-constrained **or** conflicting — resolve before closing |
| Blue | Reference (driven) dimension or construction constraint — informational only |
| Yellow | Redundant constraint — the constraint is unnecessary but not yet conflicting |

!!! tip "Trust the colour, not just the counter"
    The Solver's DoF number and the geometry colour are both produced by
    the same solver, but the colour is the authoritative signal. If the
    counter reads 0 but geometry is still white or yellow, recompute
    the sketch (close and reopen it) to force a colour refresh.

---

## Types of constraints

### Geometric constraints

Geometric constraints enforce a *relationship* between elements — no
numeric value is entered. They are satisfied as long as the relationship
holds, regardless of the actual dimensions.

| Constraint | DoF removed | What it enforces |
|------------|------------|-----------------|
| Coincident | 2 | Two points share the same location |
| Horizontal / Vertical | 1 | A line is parallel to the X or Y axis |
| Parallel | 1 | Two lines have the same direction |
| Perpendicular | 1 | Two elements meet at 90° |
| Tangent / Collinear | 1 | Smooth junction between a line and a curve |
| Equal | 1 | Two lines have the same length, or two arcs/circles the same radius |
| Symmetric | 1 | A point is the midpoint between two others (across an axis) |
| Point on Object | 1 | A point lies anywhere on an edge's support curve |
| Block | all remaining | Freezes an element in place — removes all its remaining DoF |
| Lock | 2 | Pins a point to an absolute X, Y coordinate |

### Dimensional constraints

Dimensional constraints enforce a *numeric value* — length, distance,
radius, or angle. Each one removes exactly 1 DoF.

| Constraint | DoF removed | What it sets |
|------------|------------|-------------|
| Distance / Length | 1 | Distance between two points or length of a line |
| Horizontal distance | 1 | X-component of the distance between two points |
| Vertical distance | 1 | Y-component of the distance between two points |
| Radius / Diameter | 1 | Size of a circle or arc |
| Angle | 1 | Angle between two lines, or absolute orientation |

---

## Driving vs reference mode

Every dimensional constraint can operate in one of two modes:

**Driving mode** (default)
: The value you enter *commands* the geometry. Change the value and
  the geometry moves. This is what removes the DoF.

**Reference mode** (driven)
: The constraint reads the current geometric value and *displays* it as
  a label. It does not constrain anything — it adds 0 DoF removal. The
  constraint icon turns blue. Use reference dimensions to annotate a
  sketch with measurements you want to monitor without locking them.

Toggle between modes via right-click on the constraint or the
[Toggle Driving / Reference](../tools/toggle-constraint.md) tool. A
sketch that is already fully constrained automatically offers to add
new dimensional constraints in reference mode (since there are no DoF
left to remove).

---

## Over-constraining vs conflicting constraints

Both conditions turn geometry red, but they are different problems:

| Condition | Meaning | Fix |
|-----------|---------|-----|
| **Over-constrained** | More constraints than DoF — one or more are redundant and the solver cannot satisfy all of them simultaneously | Delete the redundant constraint (usually the last one added) |
| **Conflicting** | Two constraints demand contradictory positions (e.g. a line is both horizontal and vertical) | Delete one of the conflicting constraints |

The Solver panel names the conflicting constraints when it reports an
error — use that list to identify which constraint to remove.

!!! warning "Redundant ≠ harmless"
    A yellow redundant constraint is not fatal, but it is a warning sign.
    It means a constraint you added is already implied by other constraints.
    Redundant constraints can slow the solver and occasionally cause
    numerical instability for complex sketches. Remove them.

---

## Construction geometry

Any geometry element can be toggled into **construction mode** — press
**G** or use **Sketch → Sketcher Geometries → Toggle Construction Geometry**.
Construction elements appear as thin dashed lines.

Construction geometry:
- **participates in constraints** — you can constrain construction lines
  to each other and to real geometry.
- **is excluded from the profile** — construction geometry is not passed
  to Part Design as part of the shape.

Common patterns using construction geometry:

- A **construction circle** to position bolt holes on an equal-radius pattern.
- A **construction centre line** for symmetric mirroring.
- **Construction lines connecting opposite edges** as an intermediate step
  to locate a feature's centre (see
  [Parametric Solid Modelling](../../part-design/concepts/body-and-origin.md)).

---

## External geometry

[External Geometry](../tools/external-geometry.md) pulls in edges and
vertices from outside the current sketch (from earlier features or other
bodies) as **reference-only** edges. They appear as purple dashed lines
inside the sketch.

External geometry:
- **adds 0 DoF** — it cannot be moved by the solver.
- **is not part of the profile** — like construction geometry.
- **enables parametric positioning** — a constraint tying sketch geometry
  to an external edge will update automatically if the referenced feature
  changes size.

!!! warning "External geometry indices can shift"
    Edges referenced via the Python API (`sketch.addExternal("Pad", "Edge3")`)
    are identified by topology name. When the parent feature is modified
    and recomputed, FreeCAD may rename edges (topological naming problem).
    Use the `TNP_Mitigation` settings in FreeCAD 1.0+ to reduce this risk.

---

## Practical workflow

The recommended order for constraining a new sketch:

1. **Draw all geometry** first — lines, arcs, circles — without worrying
   about exact dimensions.
2. **Apply geometric constraints**: coincident (close open wire ends),
   horizontal/vertical, tangent, parallel, equal.
3. **Apply dimensional constraints**: lock overall dimensions first
   (width, height), then locate features (centre distances, radii).
4. **Check DoF = 0** in the Solver panel and confirm all geometry is green.
5. Only then close the sketch.

Starting with geometric constraints reduces the number of dimensional
constraints needed: two lines that are declared parallel only need one
angle value to set both, rather than two separate angle values.

---

## Python API

```python
import Sketcher, math

# Geometric constraints — no value needed
sketch.addConstraint(Sketcher.Constraint("Coincident", geo1, vertex1, geo2, vertex2))
sketch.addConstraint(Sketcher.Constraint("Parallel", geo1, geo2))
sketch.addConstraint(Sketcher.Constraint("Perpendicular", geo1, geo2))
sketch.addConstraint(Sketcher.Constraint("Tangent", geo1, geo2))
sketch.addConstraint(Sketcher.Constraint("Equal", geo1, geo2))
sketch.addConstraint(Sketcher.Constraint("Symmetric", geo1, vertex1, geo2, vertex2, geoAxis))
sketch.addConstraint(Sketcher.Constraint("PointOnObject", geoPoint, vertexPoint, geoEdge))
sketch.addConstraint(Sketcher.Constraint("Horizontal", geo))
sketch.addConstraint(Sketcher.Constraint("Vertical", geo))

# Dimensional constraints — always pass values in FreeCAD base units (mm, radians)
sketch.addConstraint(Sketcher.Constraint("Distance", geo1, vertex1, geo2, vertex2, 25.0))
sketch.addConstraint(Sketcher.Constraint("Radius", circleGeo, 7.5))        # radius = diameter/2
sketch.addConstraint(Sketcher.Constraint("Angle", geo, math.radians(45)))   # UI shows degrees

# Check solver state — target is 0; negative means over-constrained
dof = sketch.solve()
print(f"Remaining DoF: {dof}")
```

!!! warning "Angles are stored in radians"
    The Sketcher UI accepts degrees, but the Python API stores and expects
    values in **radians**. Always use `math.radians(value_in_degrees)` when
    setting angle constraints programmatically.

---

## See also

- [Coincident Constraint](../tools/constraint-coincident.md) — the most fundamental geometric constraint
- [Dimension](../tools/dimension.md) — smart dimensional constraint
- [Toggle Driving / Reference](../tools/toggle-constraint.md) — switch between commanding and measuring
- [External Geometry](../tools/external-geometry.md) — referencing geometry from outside the sketch
- [Validate Sketch](../tools/validate-sketch.md) — detect and fix constraint errors
- [Sketcher Workbench](../index.md) — workbench overview
