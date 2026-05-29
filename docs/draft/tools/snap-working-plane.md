# Snap Working Plane

> **In one sentence:** Projects any snap point onto the current working plane,
> ensuring all placed points are coplanar with the working plane.

---

## Overview

**Workbench:** Draft  
**Toolbar:** Draft Snap  
**Command:** `Draft_Snap_WorkingPlane`

When enabled, snapped points that would normally be above or below the
working plane are pulled down to the plane. No separate visual indicator —
the point is projected silently.

---

## Common mistakes and pitfalls

!!! warning "Silently changes Z coordinate"
    If Working Plane snap is on and you snap to a 3-D point above the working
    plane, the placed point is projected to the working plane Z level — not at
    the height of the object you snapped to. Disable this snap if you want
    true 3-D snap positions.

---

## See also

- [Select Working Plane](select-working-plane.md) — set which plane is active
- [Snap Grid](snap-grid.md) — snap to grid on the working plane
- [Snap Lock](snap-lock.md) — master snap toggle
- [Draft Workbench](../index.md) — workbench overview
