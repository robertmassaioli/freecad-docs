# Insert Default Page

> **In one sentence:** Creates a new drawing page using the template set as
> default in Preferences — the fastest way to start a drawing.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page → Insert Default Page

The default template is set in **Edit → Preferences → TechDraw → General →
Default Template**. Out of the box it is an ISO A4 landscape template.

---

## Step-by-step

1. Choose **TechDraw → Page → Insert Default Page**.
2. A new page object appears in the model tree and the drawing editor opens.
3. The page is ready to receive views.

---

## Python API

```python
import FreeCAD as App
import TechDraw

doc = App.newDocument()
page = doc.addObject("TechDraw::DrawPage", "Page")
template = doc.addObject("TechDraw::DrawSVGTemplate", "Template")
template.Template = App.getResourceDir() + "Mod/TechDraw/Templates/A4_LandscapeUS.svg"
page.Template = template
doc.recompute()
```

---

## See also

- [Insert Page Using Template](page-insert-template.md) — choose a non-default template
- [Fill Template Fields](page-fill-fields.md) — fill in the title block
- [TechDraw Workbench](../index.md) — workbench overview
