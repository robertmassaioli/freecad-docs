# Extend Curve

> **In one sentence:** Extends an existing edge by a specified distance,
> maintaining the curve's tangent or curvature at the endpoint.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Extend Curve  
**Command:** `extend`

Two extension modes:

| Mode | Description |
|------|-------------|
| **Tangential** | Straight continuation of the tangent direction |
| **G2 smooth** | Curvature-continuous extension |

---

## Step-by-step

1. Select an edge in the 3-D view.
2. Choose **Curves → Extend Curve**.
3. Set the extension length and mode in the task panel.

---

## Common use cases

- Ensuring curves reach their intended intersection point for Gordon Surface
  or Boolean operations.
- Extending a guide rail to fully cover a sweep profile's start/end.

---

## See also

- [Join Curves](join-curves.md) — join extended curves into one
- [Gordon Surface](gordon-surface.md) — curves must intersect; extend to ensure they do
- [Curves Workbench](../index.md) — workbench overview
