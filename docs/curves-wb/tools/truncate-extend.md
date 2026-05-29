# Truncate / Extend

> **In one sentence:** Cuts a shape at a specified plane and either removes
> material beyond the plane (truncate) or extrudes the cut face to a new
> position (extend).

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Truncate/Extend  
**Command:** `Curve_TruncateExtendCmd`

The cutting plane is defined by selecting a face or datum plane.

Two modes:

- **Truncate** — removes material beyond the plane
- **Extend** — extrudes the cut face to a new position

---

## See also

- [Trim Face](trim-face.md) — trim along a projected curve rather than a plane
- [Curves Workbench](../index.md) — workbench overview
