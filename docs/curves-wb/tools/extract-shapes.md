# Extract Shapes

> **In one sentence:** Extracts individual sub-shapes from a compound or
> solid and creates independent shape objects in the model tree.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Misc. → Extract Shapes  
**Command:** `extract`

All sub-shapes are extracted simultaneously as independent objects.

---

## Common uses

- Isolate a single face from an imported solid for further surface work
- Work with individual edges from a compound wire
- Separate shells from a multi-shell solid

---

## Common mistakes and pitfalls

!!! warning "Too many objects"
    A solid or compound with many sub-faces extracts each face individually.
    Select only the sub-shape you need using the 3-D view's face selection
    mode (hover + click on the face), then extract.

---

## See also

- [Adjacent Faces](adjacent-faces.md) — identify faces to extract
- [Segment Surface](segment-surface.md) — split a surface before extracting
- [Curves Workbench](../index.md) — workbench overview
