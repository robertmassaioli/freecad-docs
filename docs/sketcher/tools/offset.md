# Offset

> **In one sentence:** Create an equidistant closed contour around selected sketch
> geometry, expanding outward for positive values and contracting inward for
> negative values.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher tools → Offset  
**Toolbar:** Sketcher Tools · **Shortcut:** `Z, T`

Offset generates a new outline that is a fixed distance from the selected edges,
following the shape of the original with parallel straight edges and rounded or
mitered corners. The result is regular sketch geometry that can be constrained and
edited like anything else in the sketch.

---

## Intuition

Think of inflating a balloon around a shape: the balloon follows the outline but
sits uniformly further out than the shape's surface. Offset does the same in 2-D:
select an outline, set the distance, and a new outline appears running parallel to
the original at every point.

Negative offset deflates (shrinks inward). The corner join style controls what
happens at sharp corners — whether the offset rounds them (Arc mode) or clips them
to a mitered point (Intersection mode).

---

## When to use it

- Designing a shell or wall of uniform thickness (offset the inner profile to
  create the outer).
- Adding an even border around a complex shape (gasket seal, clearance zone).
- Creating a tool-path offset for machining simulations in the sketch.

## When NOT to use it

- **B-Splines and conics** — Offset does not support ellipses, hyperbolas, parabolas,
  or B-Splines as input (these result in an error). Use arcs and lines instead.
- **You need a 3-D offset** — use the Part workbench Offset 3D Surface operation,
  not this Sketcher tool.

---

## Step-by-step walkthrough

1. **Select the edges** you want to offset. Use ++ctrl++-click for multiple edges.
   (Note: ellipses, parabolas, hyperbolas, and B-Splines are excluded.)

2. **Press `Z, T`** or activate **Offset** from the toolbar.

3. The offset handler appears. **Drag** the mouse to preview the offset distance
   (positive = outward, negative = inward), or **type** a value in the task panel.

4. **Choose the join type** (see deep dive below) — Arc or Intersection.

5. **Click OK** or press ++enter++. The offset geometry is added to the sketch.

6. **Constrain the offset distance** by adding a [Distance](constraint-distance.md)
   constraint between a point on the offset and the corresponding original edge.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Offset distance | Distance (mm) | Interactive drag | Positive = expand outward; negative = shrink inward. |
| Join type | Dropdown | Arc | How corners are handled: Arc (rounded) or Intersection (mitered). See deep dive below. |
| Delete original edges | Toggle | Off | If on, the original edges are removed after the offset is created. |

---

## Join type in depth

The join type controls what the offset does at sharp corners (where two edges meet
at an angle other than 180°).

#### Arc (default)

At each sharp corner, the offset inserts a circular arc connecting the two offset
segments. The arc is centred on the original corner point and has a radius equal
to the offset distance.

**Use Arc when:**
- You want smooth, rounded corners on the offset outline (e.g. a blended tool path,
  a rounded border).
- The output will be used as a profile for a Pad or Pocket where sharp corners
  might cause stress concentrations.

**Watch out for:** Very small offset distances combined with tight corners can
produce tiny arcs that may be difficult to constrain or could cause solver
problems.

#### Intersection

At each sharp corner, the offset segments are extended until they intersect,
forming a sharp mitered corner. No arc is inserted.

**Use Intersection when:**
- You need exact sharp corners on the offset (e.g. mitered frames, sheet-metal
  flanges with crisp corners).
- You plan to [Fillet](create-fillet.md) or [Chamfer](create-fillet.md#chamfer)
  the corners separately after offsetting.

**Watch out for:** At very obtuse angles, the intersection point can be far from
the original corner, creating a long spike on the offset. This is geometrically
correct but may be undesirable for the design.

---

## Advanced usage

### Combining with Trim

After an offset, use [Trim](trim-split-extend.md) to remove any unwanted segments
where the offset crosses itself (possible for concave shapes with large offset
distances).

### Inward offset for pockets

A negative offset distance creates an inward contour — useful for modelling a
pocket that has a specific wall thickness. Draw the outer profile, offset by
`−wall_thickness`, and use the inner contour as the pocket profile.

---

## Common mistakes and pitfalls

!!! warning "Offset fails with 'Selection has no valid geometries'"
    **Cause:** The selection contains B-Splines, ellipses, or other unsupported
    conic types.  
    **Fix:** Select only lines and circular arcs. Convert or approximate conic
    curves to arc/line segments before offsetting.

!!! warning "Offset self-intersects for small inward distances"
    **Cause:** A concave feature in the original profile causes the inward offset
    to cross itself.  
    **Fix:** Reduce the offset distance, or [Trim](trim-split-extend.md) the
    crossing segments after the offset is created.

---

## Python API

```python
# Offset is invoked via the GUI handler; no direct Python API is exposed
# for the interactive offset operation in FreeCAD 1.1.
# TODO: verify whether SketchObject.offset() exists as a method.
print("Use the Offset tool interactively; the result is standard geometry.")
```

---

## See also

- [Trim / Split / Extend](trim-split-extend.md) — clean up self-intersections after offset
- [Create Fillet](create-fillet.md) — add rounded corners to offset result
- [Distance](constraint-distance.md) — constrain the offset distance
