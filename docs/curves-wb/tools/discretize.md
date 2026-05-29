# Discretize

> **In one sentence:** Samples an edge or wire into an ordered array of
> points, with five sampling modes from fixed count to adaptive deflection.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Discretize  
**Command:** `Discretize`

The result is a `Points::Feature` object — an ordered array of 3-D points
along the edge.

---

## Sampling modes

| Mode | Description |
|------|-------------|
| **Number of points** | Fixed count, evenly distributed by parameter |
| **Distance** | Points separated by a specified arc-length distance |
| **Deflection** | Adaptive — more points where curvature is higher |
| **Angular deflection** | Points placed so the chord angle stays below a threshold |
| **Curve length** | Points separated by a specified chord length |

---

## Step-by-step

1. Select an edge in the 3-D view.
2. Choose **Curves → Discretize**.
3. Select the sampling mode and parameters in the task panel.
4. Click **OK**.

---

## See also

- [Approximate](approximate.md) — fit a NURBS curve back to the sampled points
- [Interpolate](interpolate.md) — create a curve passing through points
- [Curves Workbench](../index.md) — workbench overview
