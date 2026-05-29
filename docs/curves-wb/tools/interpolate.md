# Interpolate

> **In one sentence:** Creates a NURBS curve that passes exactly through each
> input point, with optional tangent constraints at the start and end.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Interpolate  
**Command:** `Interpolate`

Unlike [Approximate](approximate.md) which allows deviation, Interpolate
guarantees the curve passes through every specified point. Point tangents at
the start and end can be specified for smooth joining to adjacent curves.

---

## See also

- [Approximate](approximate.md) — approximation that allows deviation (smoother for noisy data)
- [Blend Curve](blend-curve.md) — curve connecting two edges with continuity constraints
- [Curves Workbench](../index.md) — workbench overview
