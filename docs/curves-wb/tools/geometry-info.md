# Geometry Info

> **In one sentence:** Displays geometric properties of the selected shape in
> a dockable panel — length, area, curve type, continuity, and more.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Misc. → Geometry Info  
**Command:** `GeomInfo`

Displays geometric properties in a dockable panel without switching to the
Measure workbench.

---

## Displayed properties by shape type

| Shape type | Properties shown |
|------------|-----------------|
| **Edge** | Length, curve type (line / circle / B-spline / …), degree, continuity |
| **Face** | Area, surface type, UV bounds |
| **Vertex** | XYZ coordinates |
| **Wire** | Total length, closed/open status |
| **Shell / Solid** | Volume, surface area, bounding box |

---

## See also

- [B-Spline to Console](bspline-to-console.md) — export B-spline definition to Python
- [Adjacent Faces](adjacent-faces.md) — inspect face topology
- [Curves Workbench](../index.md) — workbench overview
