# Adjacent Faces

> **In one sentence:** Highlights all faces that share an edge with the
> currently selected face — for understanding surface topology and finding
> neighbours that need blending.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Misc. → Adjacent Faces  
**Command:** `Curves_adjacent_faces`

The selected face is highlighted in one colour; its edge-sharing neighbours
in another.

---

## Common uses

- Understanding the topology of a complex shell
- Finding faces that need a blend surface between them
- Identifying disconnected shells (faces with no neighbours)

---

## See also

- [Blend Surface](blend-surface.md) — create a blend between adjacent faces
- [Geometry Info](geometry-info.md) — inspect individual face properties
- [Curves Workbench](../index.md) — workbench overview
