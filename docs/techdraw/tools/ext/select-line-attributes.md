# Select Line Attributes

> **In one sentence:** Opens a palette to choose the style, width, and colour
> that will be applied to new cosmetic lines created afterwards.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Attributes/Modifications → Select Line Attributes

This is a "set tool state" operation — it does not change existing lines,
only new ones created afterwards. The saved attributes are used by:

- Add Cosmetic Line Through 2 Points
- Draw Cosmetic Circle / Arc
- Add Parallel / Perpendicular Line

---

## Common mistakes and pitfalls

!!! warning "Set attributes before drawing new lines"
    Select Line Attributes saves a tool state. If you draw a cosmetic line
    without setting attributes first, it uses whatever state was last saved
    (or the default). Set attributes before drawing cosmetic geometry if
    you need a specific style.

---

## See also

- [Change Line Attributes](change-line-attributes.md) — apply saved attributes to existing lines
- [TechDraw Workbench](../../index.md) — workbench overview
