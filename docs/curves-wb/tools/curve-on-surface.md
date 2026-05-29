# Curve on Surface

> **In one sentence:** Creates a parametric curve constrained to lie on a
> face — defined in UV space so it updates when the surface changes.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Curve on Surface  
**Command:** `cos`

The curve is defined in the face's UV parameter space and projects onto
the 3-D surface. If the surface changes, the curve updates automatically.

---

## Common uses

- Defining a trim boundary on a face
- Creating a guide rail for a surface-following sweep
- Tracing a seam or split line on a complex surface

---

## See also

- [Sketch on Surface](sketch-on-surface.md) — map a sketch to a surface
- [Trim Face](trim-face.md) — trim a face along a projected curve
- [Curves Workbench](../index.md) — workbench overview
