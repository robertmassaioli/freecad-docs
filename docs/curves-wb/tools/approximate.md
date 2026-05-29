# Approximate

> **In one sentence:** Fits a NURBS curve or surface to a set of points,
> approximating the input within a specified tolerance.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Approximate  
**Command:** `Approximate`

Unlike [Interpolate](interpolate.md), Approximate does **not** require the
curve to pass through every point — it minimises the deviation from the point
set within the tolerance. This produces smoother curves from noisy data.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| **Degree min / max** | Polynomial degree range |
| **Max Segments** | Maximum number of knot intervals |
| **Continuity** | Internal continuity at knots: C0, C1, C2 |
| **Tolerance** | Fitting tolerance in mm |
| **Parametrization** | Chord length, centripetal, or uniform |

---

## See also

- [Interpolate](interpolate.md) — exact interpolation through every point
- [Discretize](discretize.md) — sample an edge into points to feed into Approximate
- [Curves Workbench](../index.md) — workbench overview
