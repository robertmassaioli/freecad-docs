# Circular Array

> **In one sentence:** Creates concentric rings of copies at increasing radii
> from a centre point, like ripples expanding from a stone.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools → Circular Array  
**Command:** `Draft_CircularArray`

---

## Parameters

| Property | Description |
|----------|-------------|
| Number Circles | Number of concentric rings |
| Radial Distance | Radial distance between successive rings |
| Symmetry | Number of copies per ring |
| Center | Centre point of the pattern |
| Axis | Normal to the pattern plane |

The innermost ring has `Symmetry` copies at `Radial Distance`. Outer rings
have more copies at larger radii to maintain approximate angular spacing.

---

## See also

- [Array Polar](array-polar.md) — single ring of copies
- [Array Ortho](array-ortho.md) — rectangular grid
- [Draft Workbench](../index.md) — workbench overview
