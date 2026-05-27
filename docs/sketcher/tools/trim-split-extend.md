# Trim / Split / Extend

> **In one sentence:** Modify existing sketch edges by removing the portion beyond
> an intersection (Trim), cutting an edge into two at a point (Split), or
> lengthening an edge until it meets another edge (Extend).

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Trim / Split / Extend  
**Toolbar:** Sketcher Geometries (Curve Edition dropdown) · **Shortcut:** `G, T` (Trim) · `G, Z` (Split) · `G, Q` (Extend)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Trim | `G, T` | Remove the segment of an edge on one side of an intersection |
| Split | `G, Z` | Cut an edge into two separate edges at a clicked point |
| Extend | `G, Q` | Lengthen a line or arc until it meets a target edge |

---

## Intuition

**Trim** is the sketcher equivalent of an eraser that only removes the piece you
don't want: click on the part of a line or arc that extends past an intersection,
and that excess is deleted while the rest stays.

**Split** is like scoring a piece of card with a knife: the edge stays in place
but is now two independent edges that can be constrained separately.

**Extend** is the opposite of Trim: click on the end of a line or arc and FreeCAD
projects it forward until it hits the next available edge, then stops.

---

## When to use them

- **Trim:** You have drawn a grid of lines to create a lattice, and you want to remove the interior crossing segments to leave only the outer frame.
- **Split:** You want different constraint values on two halves of a single line (e.g. a line that runs through a Boss centre — split it so you can constrain each half's length independently).
- **Extend:** You drew a line that falls short of the enclosing rectangle and want it to meet the boundary without knowing the exact endpoint coordinate.

## When NOT to use them

- **Don't use Trim when you just want to shorten a line** — adjust the endpoint position with a constraint or drag the endpoint; Trim removes geometry structurally.
- **Don't use Split when the result should be one continuous edge** — the split creates two separate objects that need a Coincident constraint to stay joined.
- **Don't use Extend if the target edge is not visible** — Extend only reaches edges that are present in the sketch; it cannot extend to external geometry.

---

## Step-by-step walkthrough

### Trim

1. **Press `G, T`** or select **Trim** from the Curve Edition dropdown.
2. **Click the segment you want to remove.** The portion of the edge on the clicked
   side of the nearest intersection is deleted. Both the trimmed edge and the
   intersecting edge remain; only the unwanted segment disappears.
3. **Repeat** for other segments to trim. Press ++esc++ to exit.

!!! tip
    Trim uses the clicked *location* to determine which segment to remove — click
    close to the end you want gone, not near the intersection.

### Split

1. **Press `G, Z`** or select **Split**.
2. **Click the point on an edge** where you want to cut it. A new vertex appears at
   that location, and the original edge becomes two edges sharing that vertex
   (connected by an automatic Coincident constraint).
3. You can now apply different constraints to each half independently.

### Extend

1. **Press `G, Q`** or select **Extend**.
2. **Click the endpoint of the edge** you want to extend (the end that should grow).
   The cursor shows a preview extending toward the nearest intersecting edge.
3. **Click again** (or just hover — the tool may auto-extend) to confirm. The edge
   grows until it meets the target.

!!! warning "Extend changes the endpoint position"
    Extending a line changes its endpoint coordinates. If the endpoint was
    previously constrained (e.g. by a coincident constraint to another vertex),
    the extension will conflict. Remove the conflicting constraint first.

---

## Parameters

All three tools are interactive-only with no numeric parameters beyond the click position. After running them, apply constraints as normal.

---

## Common mistakes and pitfalls

!!! warning "Trim removes the wrong segment"
    **Cause:** You clicked close to an intersection, and the tool picked the adjacent
    segment instead of the intended one.  
    **Fix:** Press ++ctrl+z++ to undo, then click more clearly on the target segment,
    farther from the intersection point.

!!! warning "Split leaves the sketch with more DoF than expected"
    **Cause:** Split creates two edges with a shared vertex, but any constraints on
    the original edge's midpoint are not automatically redistributed to the new
    structure.  
    **Fix:** After splitting, check the DoF counter and add any needed constraints
    to the new intermediate vertex.

!!! warning "Extend does not reach the target"
    **Cause:** The target edge is a construction element (dashed) or an external
    geometry projection, and Extend may not recognize it as a valid endpoint.  
    **Fix:** Convert the target to a regular edge, or manually extend by dragging
    the endpoint and adding a Coincident constraint.

---

## Python API

### Trim

```python
import FreeCAD as App
import Sketcher

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Trim geo index 2 at the point closest to (15, 0)
sketch.trim(2, App.Vector(15, 0, 0))
doc.recompute()
```

### Split

```python
# Split geo index 1 at parameter value 0.5 (midpoint)
sketch.split(1, App.Vector(5, 5, 0))  # point on the edge
doc.recompute()
```

### Extend

```python
# Extend the endpoint (vertex 2) of geo 0 toward geo 1
sketch.extend(0, 0.5, 2)  # geoId, parameter, endPoint (1=start, 2=end)
doc.recompute()
```

---

## See also

- [Create Line](create-line.md) — draw lines to trim/extend
- [Create Arc](create-arc.md) — draw arcs to trim
- [External Geometry](external-geometry.md) — project external edges that can serve as Extend targets
- [Coincident](constraint-coincident.md) — reconnect split edge halves to other geometry
