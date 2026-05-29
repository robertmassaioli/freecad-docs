# Draft to Sketch

> **In one sentence:** Converts Draft objects to Sketcher sketch objects
> (or vice versa) so geometry can move between the two environments.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Draft to Sketch  
**Command:** `Draft_Draft2Sketch`

**Draft → Sketch:** Apply Sketcher constraints to geometry originally drawn
freehand in Draft.

**Sketch → Draft:** Extract geometry from a parametric sketch into the Draft
environment (e.g. for arrays or TechDraw views).

---

## Common mistakes and pitfalls

!!! warning "Not all geometry converts losslessly"
    Complex B-splines may be approximated, and construction geometry is lost
    during conversion. Check the result after conversion.

---

## See also

- [Wire to B-Spline](wire-to-bspline.md) — convert between Draft wire types
- [Draft Workbench](../index.md) — workbench overview
