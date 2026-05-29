# Join Curves

> **In one sentence:** Joins multiple selected edges into a single continuous
> B-spline curve.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Join Curves  
**Command:** `join`

The result is a single B-spline that passes through the endpoints of all
input edges in order. Useful for converting a chain of separate edges (e.g.
from imported geometry) into one smooth parametric curve.

---

## Step-by-step

1. Select the edges to join (in the correct order).
2. Choose **Curves → Join Curves**.
3. A single B-spline replaces the chain of edges.

Alternatively, select an object containing multiple edges in the tree — the
tool joins them automatically.

---

## Common mistakes and pitfalls

!!! warning "Join Curves produces a coarse approximation"
    The joining algorithm approximates to a B-spline which may have fewer
    degrees of freedom than the input chain. Increase **Max Degree** or
    **Max Segments** if available, or use [Approximate](approximate.md) with
    a tighter tolerance.

---

## See also

- [Split Curve](split-curve.md) — split a curve at one or more points
- [Approximate](approximate.md) — fit a NURBS curve to a point set
- [Curves Workbench](../index.md) — workbench overview
