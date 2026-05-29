# Trim Face

> **In one sentence:** Trims a face using a curve projected onto it —
> removing the portion on the selected side of the projection.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Trim Face  
**Command:** `Trim`

The curve is projected onto the face along a specified direction, and the
portion of the face on the selected side of the projection is removed.

---

## Step-by-step

1. Select the face to trim.
2. Select the cutter curve (edge or wire).
3. Choose **Surfaces → Trim Face**.
4. In the task panel, choose the projection direction and which side to keep.

---

## See also

- [Curve on Surface](curve-on-surface.md) — create a UV-space curve as a trim boundary
- [Segment Surface](segment-surface.md) — divide a surface along iso-parameter lines
- [Curves Workbench](../index.md) — workbench overview
