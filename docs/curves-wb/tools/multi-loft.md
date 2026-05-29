# Multi-Loft

> **In one sentence:** Creates a loft surface through a sequence of profiles
> where each profile may be composed of multiple faces rather than a single
> wire.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Multi-Loft  
**Command:** `MultiLoft`

This extends the standard Part Loft by accepting profile objects made of faces
(compound solids or shells) rather than just wires or edges. Suitable for
lofting between cross-sections with interior structure.

---

## See also

- [Gordon Surface](gordon-surface.md) — surface from a full curve network
- [Sweep 2 Rails](sweep-2-rails.md) — sweep along two guide rails
- [Curves Workbench](../index.md) — workbench overview
