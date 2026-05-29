# Export Page as SVG

> **In one sentence:** Exports the active drawing page — views, dimensions,
> annotations, and template — to a standard SVG file for use in Inkscape,
> Illustrator, or a web browser.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page → Export Page as SVG

The exported SVG is a standard W3C SVG 1.1 file. It captures a static
snapshot of the drawing at the moment of export.

---

## What is exported

- The template border and title block (as vector artwork)
- All views (projected as SVG paths)
- All dimensions and annotation text
- Hatching patterns

## What is not exported

- FreeCAD-specific metadata (dimension links, model references)
- The 3-D model geometry itself

---

## Step-by-step

1. Make the page active or select it in the model tree.
2. Choose **TechDraw → Page → Export Page as SVG**.
3. Choose a file path. Click **Save**.

---

## Python API

```python
import TechDrawGui

page = doc.getObject("Page")
TechDrawGui.exportPageAsSVG(page, "/path/to/drawing.svg")
```

---

## Common mistakes and pitfalls

!!! warning "SVG export does not round-trip back to FreeCAD"
    The exported SVG is a static snapshot. Opening it in FreeCAD does not
    restore the parametric TechDraw document — it shows only flat vector
    artwork. Keep the `.FCStd` file as the master and export SVG as needed.

---

## See also

- [Export Page as DXF](page-export-dxf.md) — export for AutoCAD / 2-D CAD systems
- [Print All Pages](page-print-all.md) — send pages to the printer
- [TechDraw Workbench](../index.md) — workbench overview
