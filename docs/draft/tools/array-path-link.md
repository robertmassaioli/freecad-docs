# Path Link Array

> **In one sentence:** Like Path Array but uses App::Link instances instead of
> full copies — lighter on memory for large arrays.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools → Path Link Array  
**Command:** `Draft_PathLinkArray`

Identical to [Path Array](array-path.md) in all parameters and behaviour,
but each element is an `App::Link` pointing to a single shared shape.
Preferred for arrays with 50+ elements.

---

## See also

- [Array Path](array-path.md) — standard (non-link) variant
- [Array Point Link](array-point-link.md) — link array at point positions
- [Draft Workbench](../index.md) — workbench overview
