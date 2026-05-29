# Hatch

> **In one sentence:** Fills a closed Draft shape with an SVG or AutoCAD PAT
> hatch pattern for display in the 3-D view.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Hatch  
**Command:** `Draft_Hatch`

Fills a closed wire, rectangle, circle, or polygon with a repeating hatch
pattern. The shape must be closed. Hatch is display-only — it has no solid
geometry.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| File | SVG or PAT file containing the pattern |
| Pattern | Name of the pattern within the file |
| Scale | Scale factor for the pattern spacing |
| Rotation | Rotation angle of the pattern (degrees) |

---

## Notes

- FreeCAD ships with a set of bundled SVG patterns.
- Custom `.pat` files (AutoCAD format) can be loaded from disk.
- For TechDraw output, use TechDraw's built-in Hatch tools instead.

---

## Common mistakes and pitfalls

!!! warning "Shape must be closed"
    Hatch only works on a closed wire or face. If the wire is not closed,
    the hatch cannot be applied.

---

## See also

- [Wire](wire.md) — closed wire to hatch
- [Rectangle](rectangle.md) — hatch a rectangular area
- [Draft Workbench](../index.md) — workbench overview
