# Segment Surface

> **In one sentence:** Divides a surface into segments along iso-parameter
> lines at specified U and V values, producing a compound of sub-faces.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Segment Surface  
**Command:** `segment_surface`

---

## Step-by-step

1. Select a face.
2. Choose **Surfaces → Segment Surface**.
3. In the task panel, add U or V split parameters (0–1).
4. Each split produces an additional sub-face in the compound.

---

## See also

- [Trim Face](trim-face.md) — trim with a projected curve instead of iso-parameter lines
- [Extract Shapes](extract-shapes.md) — extract resulting sub-faces for further work
- [Curves Workbench](../index.md) — workbench overview
