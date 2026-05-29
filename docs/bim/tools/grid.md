# Grid

> **In one sentence:** Create a persistent 2-D rectangular grid document
> object with specified rows, columns, and spacing.

---

## Overview

**Workbench:** BIM  
**Menu:** Annotation → Grid  
**Command:** `Arch_Grid`  
**IFC class:** `IfcGrid`

Creates a 2-D rectangular grid object. Unlike the Draft working-plane
background grid (which is display-only), this Grid is a persistent document
object with specified row count, column count, and spacing — it exports to
IFC as `IfcGrid`.

---

## When to use it

- Structural column grids that need to export to IFC.
- Reference grids for laying out repetitive elements.

## When NOT to use it

- **For a column grid from individual reference lines** — use [Axis](axis.md)
  and [Axis System](axis-system.md) for more flexibility.

---

## See also

- [Axis](axis.md) — individual axis line
- [Axis System](axis-system.md) — grid from multiple Axis objects
- [BIM Workbench](../index.md) — workbench overview
