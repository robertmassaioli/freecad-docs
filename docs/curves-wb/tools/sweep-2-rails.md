# Sweep 2 Rails

> **In one sentence:** Sweeps a profile curve along two guide rails, producing
> a surface whose edges lie exactly on both rails.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Sweep 2 Rails  
**Command:** `sw2r`

The profile shape changes along the sweep to connect the two rails at each
point. The rails and profile must share endpoints (the profile endpoints must
touch both rails at the same U parameter).

---

## Selection order

1. First rail (edge or wire)
2. Second rail (edge or wire)
3. Profile (edge or wire) — connects the two rails at the start

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| **Continuity** | Continuity at the start / end profile: G0, G1, G2 |
| **Scale** | Scale factor for the profile shape along the sweep |

---

## See also

- [Pipeshell](pipeshell.md) — sweep with explicit orientation control modes
- [Gordon Surface](gordon-surface.md) — surface from a full crossing-curve network
- [Curves Workbench](../index.md) — workbench overview
