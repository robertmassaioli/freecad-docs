# Export Page as DXF

> **In one sentence:** Exports the active drawing page as a DXF file for
> import into AutoCAD, LibreCAD, or other 2-D CAD systems.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page → Export Page as DXF

DXF (Drawing Exchange Format) is the standard interchange format for 2-D CAD.
The exported file is a flat 2-D representation of the drawing.

---

## Notes on DXF export

- Text is exported as DXF TEXT or MTEXT entities.
- Dimensions are exported as DXF DIMENSION entities where possible.
- The template border is included as DXF geometry.
- DXF export is a flat 2-D representation — no 3-D data is included.

---

## Python API

```python
import TechDrawGui

page = doc.getObject("Page")
TechDrawGui.exportPageAsDXF(page, "/path/to/drawing.dxf")
```

---

## Common mistakes and pitfalls

!!! warning "DXF text encoding"
    DXF files use a code-page encoding that may not support all Unicode
    characters. Special symbols (°, ∅, ±) may need to be entered using DXF
    control codes (`%%d`, `%%c`, `%%p`) if the downstream CAD tool does not
    support Unicode DXF.

---

## See also

- [Export Page as SVG](page-export-svg.md) — export as vector SVG
- [Print All Pages](page-print-all.md) — send pages to the printer
- [TechDraw Workbench](../index.md) — workbench overview
