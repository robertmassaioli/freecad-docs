# Insert Page Using Template

> **In one sentence:** Creates a new drawing page by opening a file browser
> to choose any SVG template — for non-default paper sizes and company templates.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page → Insert Page Using Template

FreeCAD ships templates in `<install>/Mod/TechDraw/Templates/`:

| Template | Size | Orientation |
|----------|------|-------------|
| `A4_LandscapeUS.svg` | A4 | Landscape, ANSI-style |
| `A4_Portrait_ISO7200.svg` | A4 | Portrait, ISO 7200 |
| `A3_Landscape_ISO7200.svg` | A3 | Landscape, ISO 7200 |
| `A2_Landscape_ISO7200.svg` | A2 | Landscape, ISO 7200 |
| `A1_Landscape_ISO7200.svg` | A1 | Landscape, ISO 7200 |
| `A0_Landscape_ISO7200.svg` | A0 | Landscape, ISO 7200 |
| `USLetter_Landscape.svg` | US Letter | Landscape |

### Custom templates

A TechDraw template is any SVG file. To make title-block fields editable
from FreeCAD, add `<text>` elements with the attribute
`freecad:editable="FieldName"`.

---

## Common mistakes and pitfalls

!!! warning "Switching templates discards field values"
    If you change the template of an existing page, any previously filled
    field values are discarded because the new template may have different
    field names. Re-fill the fields after switching.

---

## See also

- [Insert Default Page](page-insert-default.md) — one-click page with default template
- [Fill Template Fields](page-fill-fields.md) — fill in the title block
- [TechDraw Workbench](../index.md) — workbench overview
