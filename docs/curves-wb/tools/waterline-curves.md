# Waterline Curves

> **In one sentence:** Generates horizontal cross-section contours
> (waterlines) on a surface or solid at a series of Z heights.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Waterline Curves  
**Command:** `Curves_WaterlineCurves`

Each waterline is the intersection of a horizontal plane with the shape at a
given Z height. The result is a set of edges tracing these intersections.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| **Number of levels** | How many horizontal cross-sections to generate |
| **Z min / Z max** | Height range |
| **Step** | Spacing between levels (alternative to count) |

---

## Common uses

- Visualising 3-D hull or terrain shapes as 2-D contours
- Generating level curves for milling toolpaths
- Checking a shape's cross-sectional profile at multiple heights

---

## See also

- [IsoCurve](isocurve.md) — iso-parameter curves in UV space rather than Z height
- [Curves Workbench](../index.md) — workbench overview
