# Lock Position Constraint

> **In one sentence:** Fix a vertex at absolute X and Y coordinates in the sketch,
> removing both its translational degrees of freedom in one step.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Lock Position  
**Toolbar:** Sketcher Constraints · **Shortcut:** `K, L`

Lock Position applies a horizontal distance and a vertical distance constraint from
the sketch origin to the selected vertex simultaneously. The result is that the
vertex is pinned to a specific (X, Y) coordinate. It is equivalent to manually
adding `DistanceX` and `DistanceY` constraints with the same values.

---

## Intuition

Think of pressing a pin through a drawing into the board at an exact measured
position: X mm from the left edge, Y mm from the bottom. Lock Position is that
pin — the vertex cannot drift because both its horizontal and vertical positions
are specified absolutely.

---

## When to use it

- Anchoring one corner of a profile to the sketch origin (set X=0, Y=0).
- Pinning a bolt-hole centre to a precisely known coordinate.
- Fully constraining a standalone point used as a layout reference.
- Fixing the position of one key feature before constraining others relative to it.

## When NOT to use it

- **You want the vertex to follow another vertex's position** — use
  [Coincident](constraint-coincident.md) instead.
- **You want to fix only one coordinate (X or Y) but leave the other free** — use
  a single [Horizontal Distance](constraint-distance.md#horizontal-distance) or
  [Vertical Distance](constraint-distance.md#vertical-distance) constraint.
- **You want the geometry frozen regardless of its coordinates** — use
  [Block](constraint-block.md) for a quick non-parametric freeze.

---

## Step-by-step walkthrough

1. **Select a vertex** (a line endpoint, arc endpoint, or standalone point).
2. **Press `K, L`** or activate **Sketch → Lock Position**.
3. A value dialog appears with the current X and Y coordinates pre-filled.
4. **Edit the X and Y values** to the desired absolute coordinates.
5. **Click OK.** Two constraints appear in the Constraints panel:
   a horizontal distance from the origin and a vertical distance from the origin.

!!! tip
    To anchor a vertex to the sketch origin exactly, select it and apply Lock
    Position, then set both X and Y to 0. Or use
    [Coincident](constraint-coincident.md) with the origin point — it is equivalent
    and uses fewer constraints.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| X | Distance | current X | Horizontal distance from the sketch origin to the vertex. May be negative. |
| Y | Distance | current Y | Vertical distance from the sketch origin to the vertex. May be negative. |

---

## Common mistakes and pitfalls

!!! warning "Lock Position creates redundant constraints"
    **Cause:** The vertex was already constrained by other means (e.g. Coincident
    with the origin, or two Distance constraints from other elements).  
    **Fix:** Resolve the redundancy by deleting one of the competing constraints.
    The solver highlights them in red.

!!! warning "Lock Position on an interior vertex of a polyline over-constrains it"
    **Cause:** Interior vertices of a polyline are already constrained by Coincident
    with the adjacent edges; pinning them with Lock Position on top is redundant.  
    **Fix:** Apply Lock Position only to *free* vertices (endpoints, standalone
    points). Constrain interior vertices with Distance constraints relative to
    adjacent geometry.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

idx = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(20, 10, 0)), False)

# Fix the start point at (5, 3)
sketch.addConstraint(Sketcher.Constraint("DistanceX", idx, 1, -1, 1, 5.0))
sketch.addConstraint(Sketcher.Constraint("DistanceY", idx, 1, -1, 1, 3.0))
doc.recompute()
```

### Method signatures

```python
# DistanceX from origin to vertex (idx, vertex_num) — positive = rightward
sketch.addConstraint(Sketcher.Constraint("DistanceX", geoId, vertex, -1, 1, x_value))

# DistanceY from origin to vertex — positive = upward
sketch.addConstraint(Sketcher.Constraint("DistanceY", geoId, vertex, -1, 1, y_value))
```

---

## See also

- [Coincident](constraint-coincident.md) — place a vertex at another vertex (including the origin)
- [Distance](constraint-distance.md) — set relative distances between two elements
- [Block](constraint-block.md) — freeze geometry without specifying coordinates
- [Dimension](dimension.md) — smart constraint that auto-detects the appropriate type
