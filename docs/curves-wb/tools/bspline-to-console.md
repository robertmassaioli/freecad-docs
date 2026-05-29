# B-Spline to Console

> **In one sentence:** Exports the full definition of a selected B-spline
> edge — poles, knot vector, weights, and degree — to the Python console as
> reconstructable code.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Misc. → B-Spline to Console  
**Command:** `Curves_bspline_to_console`

The output is formatted as Python code that can reconstruct the curve:

```python
import Part
bs = Part.BSplineCurve()
bs.buildFromPolesMultsKnots(
    poles=[...],
    mults=[...],
    knots=[...],
    periodic=False,
    degree=3,
    weights=[...],
    CheckRational=True
)
```

---

## What is exported

- **Control points** — XYZ coordinates of every pole
- **Knot vector** — the full non-uniform knot sequence
- **Weights** — rational weights (for NURBS)
- **Degree**

---

## When to use it

- Archive a manually designed B-spline for reuse in scripts.
- Debug a curve that behaves unexpectedly by examining its knot structure.
- Transfer control point data to an external CAD or analysis application.

---

## See also

- [To Console](to-console.md) — general shape properties to console
- [Geometry Info](geometry-info.md) — interactive panel for quick inspection
- [Curves Workbench](../index.md) — workbench overview
