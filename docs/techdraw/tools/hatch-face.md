# Hatch a Face

> **In one sentence:** Fills a closed face in a view with a repeating SVG
> hatch pattern — for section hatching and material-specific patterns.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Hatching → Hatch a Face

This is the standard way to add section hatching (cross-hatching) to cut
faces in section views, and to indicate material with standard hatch patterns.

---

## Step-by-step

1. Click a closed face in a view (it highlights).
2. Choose **TechDraw → Hatching → Hatch a Face**.
3. In the task panel, select an SVG hatch pattern file.
4. Set the scale and rotation of the pattern.
5. Click **OK**.

---

## Bundled SVG hatch patterns

FreeCAD ships standard hatch patterns in `<FreeCAD install>/Mod/TechDraw/Patterns/`.

| File | Pattern |
|------|---------|
| `simple.svg` | Simple diagonal lines (ISO general hatching) |
| `steel.svg` | Steel (diagonal, 45°) |
| `aluminium.svg` | Aluminium (two sets of lines) |
| `wood.svg` | Wood grain |
| `concrete.svg` | Concrete (dot and dash) |

---

## Properties

| Property | Description |
|----------|-------------|
| HatchPattern | Path to the SVG pattern file |
| HatchScale | Scale factor for the pattern repeat |
| HatchRotation | Rotation of the pattern in degrees |
| HatchOffset | Offset of the pattern origin |
| HatchColor | Override the pattern colour |

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
page = doc.getObject("Page")
view = doc.getObject("SectionView")

hatch = doc.addObject("TechDraw::DrawHatch", "Hatch1")
hatch.Source = (view, ["Face1"])
hatch.HatchPattern = App.getResourceDir() + "Mod/TechDraw/Patterns/simple.svg"
hatch.HatchScale = 10.0
page.addView(hatch)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Hatch pattern file paths are absolute"
    If you move the `.FCStd` file to another computer, hatch patterns stored
    with absolute paths may not be found. Use patterns from the bundled FreeCAD
    Patterns directory (accessed via relative `App.getResourceDir()`) rather
    than patterns stored in arbitrary locations.

---

## See also

- [Apply Geometric Hatch](hatch-geometric.md) — PAT file hatching (AutoCAD-compatible)
- [Insert Section View](view-section.md) — section views with auto-hatching
- [TechDraw Workbench](../index.md) — workbench overview
