# Path Twisted Array

> **In one sentence:** Like Path Array with an added twist — each copy is
> incrementally rotated along the path, producing a helical arrangement.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools → Path Twisted Array  
**Command:** `Draft_PathTwistedArray`

All [Path Array](array-path.md) parameters apply, plus:

| Property | Description |
|----------|-------------|
| Twist Start | Rotation angle of the first element (degrees) |
| Twist End | Rotation angle of the last element (degrees) |

Intermediate elements are interpolated linearly between Twist Start and
Twist End.

---

## Use cases

- Twisted column arrangements in architecture.
- Decorative spiral patterns.
- Helical blade arrangements (for precise structural helices, use Part Helix).

---

## See also

- [Array Path Twisted Link](array-path-twisted-link.md) — link-instance variant
- [Array Path](array-path.md) — path array without twist
- [Draft Workbench](../index.md) — workbench overview
