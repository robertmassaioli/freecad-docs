# Sketch on Surface

> **In one sentence:** Maps a flat Sketcher sketch onto a curved surface —
> wrapping the XY plane onto the target face's UV parametric space.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Sketch on Surface  
**Command:** `SoS`

The sketch's XY extent is mapped to the face's full UV range (0–1 in both
directions). The result updates if the underlying face changes.

---

## Common uses

- Engraving lettering or logos onto a curved part
- Projecting a 2-D pattern onto a free-form surface
- Creating trim curves that follow a surface's shape

---

## Common mistakes and pitfalls

!!! warning "Sketch must lie flat in XY plane"
    Any Z offset in the sketch will be ignored. All geometry must be at Z = 0.

!!! warning "UV distortion is inherent"
    A non-uniform UV parametrisation means equal XY distances in the sketch
    map to unequal arc lengths on the surface. Accept this distortion or use
    [Curve on Surface](curve-on-surface.md) to manually trace the face's geometry.

---

## See also

- [Curve on Surface](curve-on-surface.md) — manually place curves by tracing the face
- [Map on Face](map-on-face.md) — map arbitrary edges (not just sketches)
- [Curves Workbench](../index.md) — workbench overview
