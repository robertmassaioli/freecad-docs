# ShapeString

> **In one sentence:** Creates a compound of closed wire outlines from a text
> string using a TrueType or OpenType font — geometry that can be extruded
> into 3-D lettering.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → ShapeString  
**Command:** `Draft_ShapeString`

The result is solid-quality geometry (wires, not a bitmap) that can be
extruded with **Part Design → Pad** or **Part → Extrude** to make raised or
engraved 3-D text.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| String | The text to convert |
| Font file | Path to a .ttf or .otf font file |
| Size | Character height (in mm, model units) |
| Justification | Left / Right / Centre alignment |

---

## Workflow: 3-D text

```
1. Place ShapeString on the working plane.
2. Switch to Part workbench.
3. Select the ShapeString, then Part → Extrude.
4. Set direction and depth to create 3-D letters.
```

---

## Python API

```python
import Draft

ss = Draft.make_shapestring(
    String="FreeCAD",
    FontFile="/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf",
    Size=10.0,
    Tracking=0
)
ss.Placement.Base = App.Vector(0, 0, 0)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Font path is absolute"
    The font file path is stored as an absolute path. If you move the document
    to a different machine, the font reference will break. Use a font available
    on both machines, or embed a copy alongside the document.

---

## See also

- [Draft Workbench](../index.md) — workbench overview
