# Parametric Line

> **In one sentence:** Creates a parametric line between two selected vertices
> that automatically updates if either vertex moves.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Parametric Line  
**Command:** `Curves_line`

Unlike a Draft Line, this line is parametrically linked to its endpoint
vertices — if a vertex moves (because the shape it belongs to changes),
the line updates automatically.

---

## Step-by-step

1. Select 2 vertices in the 3-D view.
2. Choose **Curves → Parametric Line**.
3. The line appears and is linked to both vertices.

---

## See also

- [Join Curves](join-curves.md) — join multiple edges into a single B-spline
- [Curves Workbench](../index.md) — workbench overview
