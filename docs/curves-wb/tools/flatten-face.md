# Flatten Face

> **In one sentence:** Unrolls a conical or cylindrical face into a flat 2-D
> sheet, preserving arc lengths along every profile curve.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Flatten Face  
**Command:** `Curves_FlattenFace`

The unrolled shape has the same arc length along every profile curve as the
original 3-D face. Used to generate flat patterns for sheet material that
will be bent or wrapped.

---

## Common uses

- Flat patterns for sheet metal bent around a cylinder
- Fabric or veneer patterns that will be wrapped onto a surface

---

## Common mistakes and pitfalls

!!! warning "Only works on developable surfaces"
    Flatten Face requires a surface with zero Gaussian curvature (cone,
    cylinder, or their trimmed subsets). Attempting to flatten a doubly-curved
    surface (sphere, free-form NURBS) will produce distorted or incorrect
    results.

---

## See also

- [Surface Analysis](surface-analysis.md) — check Gaussian curvature to confirm developability
- [Curves Workbench](../index.md) — workbench overview
