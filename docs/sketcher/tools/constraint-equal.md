# Equal Constraint

> **In one sentence:** Force two edges to have the same length (for lines) or
> the same radius (for circles and arcs), without specifying what that value is.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Equal Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `E`

Equal removes 1 degree of freedom by locking the length/radius of one element to
match another. It does not set the actual value — that is left to a separate
dimensional constraint or derived from other geometry.

---

## Intuition

Think of a balance scale: Equal says the two pans weigh the same without specifying
what that weight is. One length or radius becomes the "master" and the other is
forced to match it. Set the master's value with one constraint, and the equal one
follows for free.

---

## When to use it

- All four sides of a square: apply Equal between adjacent sides so that a single
  [Distance](constraint-distance.md) constraint sets the overall size.
- All radii of a rounded rectangle's corners: one [Radius/Diameter](constraint-radius-diameter.md)
  constraint applies to all four corner arcs.
- Two circles that must always be the same size (e.g. two bolt holes of the same
  diameter, positioned independently).
- The two legs of an isosceles triangle.
- All segments of a regular polygon (the polygon tool applies Equal automatically,
  but manual polygons need it added).

## When NOT to use it

- **You want both edges to have a *specific* known value** — set each with its own
  dimensional constraint; Equal is only needed when you want them coupled without
  specifying both.
- **The elements are different types** — Equal between a line and a circle radius
  is not defined; only line-to-line (length) and arc/circle-to-arc/circle (radius)
  comparisons are valid.

---

## Step-by-step walkthrough

1. **Select two lines or two circles/arcs** — ++ctrl++-click to select both.
2. **Press `E`** or activate **Sketch → Equal Constraint**.
3. One element adjusts its length/radius to match the other. The solver decides
   which one moves based on which has fewer constraints.

You can also select more than two elements at once; Equal will chain all of them
together.

---

## Parameters

No numeric parameters. Removes 1 DoF per pair linked.

---

## Common mistakes and pitfalls

!!! warning "Equal applied between a line and a circle"
    **Cause:** You selected a line and a circle; Equal cannot compare their sizes
    (length vs radius are different quantities).  
    **Fix:** Select two lines, or two circles/arcs. Use separate constraints for
    each type.

!!! warning "Adding Equal after setting both sizes individually causes over-constraint"
    **Cause:** Both elements had their length/radius independently constrained, and
    adding Equal makes one of those constraints redundant.  
    **Fix:** Remove one of the individual dimensional constraints (keep the one on
    the "master" element) and then apply Equal.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Two lines that must have equal length
line0 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(15, 0, 0)), False)
line1 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 10, 0), App.Vector(20, 10, 0)), False)

sketch.addConstraint(Sketcher.Constraint("Equal", line0, line1))
# Now set the master length:
sketch.addConstraint(Sketcher.Constraint("Distance", line0, 15.0))
doc.recompute()
# line1 also becomes 15 mm long.
```

---

## See also

- [Symmetric](constraint-symmetric.md) — related constraint for mirror symmetry
- [Radius/Diameter](constraint-radius-diameter.md) — set the actual radius value
- [Distance](constraint-distance.md) — set the actual length value
- [Parallel](constraint-parallel.md) — force equal direction (complements Equal for parallelograms)
