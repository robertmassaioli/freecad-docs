# Pages

> **In one sentence:** A TechDraw Page is the drawing sheet — create one
> from a template, fill in the title block, and export it to SVG or DXF
> when finished.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page

| Tool | Description |
|------|-------------|
| [Insert Default Page](#insert-default-page) | New page using the default template |
| [Insert Page Using Template](#insert-page-using-template) | New page from a chosen SVG template |
| [Fill Template Fields](#fill-template-fields) | Edit title-block text fields |
| [Redraw Page](#redraw-page) | Force all views to regenerate |
| [Print All Pages](#print-all-pages) | Print every drawing page in the document |
| [Export Page as SVG](#export-page-as-svg) | Save the page as an SVG file |
| [Export Page as DXF](#export-page-as-dxf) | Save the page as a DXF file |

---

## Intuition

Every TechDraw drawing starts with a **Page**. A page is a container that
holds views, dimensions, and annotations. It is always backed by a
**Template** — an SVG file that provides the physical drawing border, title
block columns, company logo area, and any other fixed artwork that appears
on every sheet.

The separation of *page* (variable content: views, dimensions) from
*template* (fixed artwork: border, title block layout) means you can:

- Switch to a different paper size or company standard by swapping the
  template without touching the views.
- Fill in the title block fields (title, drawn by, date, revision) via
  FreeCAD's UI — no SVG editor required.
- Maintain multiple pages in one document (assembly drawing, detail sheets)
  each with the same or different templates.

---

## Insert Default Page

**Menu:** TechDraw → Page → Insert Default Page

Creates a new page using whichever template is set as the default in
**Edit → Preferences → TechDraw → General → Default Template**. Out of
the box, the default is an ISO A4 landscape template.

### Step-by-step

1. Choose **TechDraw → Page → Insert Default Page**.
2. A new page object appears in the model tree and the drawing editor
   opens automatically.
3. The page is ready to receive views.

### Python API

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

## Insert Page Using Template

**Menu:** TechDraw → Page → Insert Page Using Template

Opens a file browser to choose any SVG file as the template. Use this when
you need a non-default paper size, a company template, or an ISO vs ANSI
variant.

### Bundled templates

FreeCAD ships templates in:

```
<FreeCAD install>/Mod/TechDraw/Templates/
```

They follow the naming pattern `<Size>_<Orientation>[US].svg`:

| Template | Size | Orientation |
|----------|------|-------------|
| `A4_LandscapeUS.svg` | A4 | Landscape, ANSI-style |
| `A4_Portrait_ISO7200.svg` | A4 | Portrait, ISO 7200 title block |
| `A3_Landscape_ISO7200.svg` | A3 | Landscape, ISO 7200 |
| `A2_Landscape_ISO7200.svg` | A2 | Landscape, ISO 7200 |
| `A1_Landscape_ISO7200.svg` | A1 | Landscape, ISO 7200 |
| `A0_Landscape_ISO7200.svg` | A0 | Landscape, ISO 7200 |
| `USLetter_Landscape.svg` | US Letter | Landscape |

### Custom templates

A TechDraw template is any SVG file. To make title-block fields editable
from FreeCAD, add `<text>` elements with the attribute
`freecad:editable="FieldName"`. FreeCAD exposes these as the editable fields
in Fill Template Fields.

---

## Fill Template Fields

**Menu:** TechDraw → Page → Fill Template Fields

Opens a dialog listing all editable text fields defined in the active page's
template. Type the values you want — title, scale, drawn by, date, revision
number, and so on.

### Step-by-step

1. Select the page in the model tree (or make sure a drawing page is open).
2. Choose **TechDraw → Page → Fill Template Fields**.
3. A dialog lists all editable field names found in the template SVG.
4. Type the value for each field.
5. Click **OK**. The title block text updates instantly on the page.

### Common standard fields (ISO 7200)

| Field | Typical content |
|-------|----------------|
| `FC-Title` | Drawing title |
| `FC-Scale` | Drawing scale (e.g. `1:2`) |
| `FC-Author` | Designer's name |
| `FC-Date` | Date |
| `FC-Organisation` | Company or organisation name |
| `FC-Sheet` | Sheet number (e.g. `1/3`) |
| `FC-Rev` | Revision letter or number |
| `FC-DrawingNumber` | Document / part number |

Exact field names depend on the template. The dialog shows only the fields
that are actually in the current template.

### Python API

```python
page = doc.getObject("Page")
template = page.Template

# List all editable fields
fields = template.EditableTexts   # returns a dict {fieldName: currentValue}

# Set values
fields["FC-Title"] = "Bracket Assembly"
fields["FC-Author"] = "R. Massaioli"
fields["FC-Date"] = "2026-05-28"
template.EditableTexts = fields
doc.recompute()
```

---

## Redraw Page

**Menu:** TechDraw → Page → Redraw Page

Forces all views on the active page to recompute their projected geometry.
TechDraw normally updates views automatically on recompute, but occasionally
a view can become stale after a model change that did not trigger a full
recompute. Use Redraw Page to force a refresh.

---

## Print All Pages

**Menu:** TechDraw → Page → Print All Pages

Sends every TechDraw page in the document to the system print dialog in
one operation. Useful for multi-sheet drawing packages.

For individual pages, use **File → Print** with the drawing page active, or
right-click the page in the model tree and choose Print.

---

## Export Page as SVG

**Menu:** TechDraw → Page → Export Page as SVG

Exports the active drawing page — all views, dimensions, annotations, and
the template border — to a single SVG file. The exported SVG is a standard
W3C SVG 1.1 file that can be opened in Inkscape, Adobe Illustrator, or any
modern web browser.

### What is exported

- The template border and title block (as vector artwork)
- All views (projected as SVG paths)
- All dimensions and annotation text
- Hatching patterns

### What is not exported

- FreeCAD-specific metadata (dimension links, model references)
- The 3-D model geometry itself

### Step-by-step

1. Make the page active or select it in the model tree.
2. Choose **TechDraw → Page → Export Page as SVG**.
3. Choose a file path. Click **Save**.

### Python API

```python
import TechDrawGui

page = doc.getObject("Page")
TechDrawGui.exportPageAsSVG(page, "/path/to/drawing.svg")
```

---

## Export Page as DXF

**Menu:** TechDraw → Page → Export Page as DXF

Exports the active drawing page as a DXF (Drawing Exchange Format) file for
import into AutoCAD, LibreCAD, or other 2-D CAD systems.

### Notes on DXF export

- Text is exported as DXF TEXT or MTEXT entities.
- Dimensions are exported as DXF DIMENSION entities where possible.
- The template border is included as DXF geometry.
- DXF export is a flat 2-D representation — no 3-D data is included.

### Python API

```python
import TechDrawGui

page = doc.getObject("Page")
TechDrawGui.exportPageAsDXF(page, "/path/to/drawing.dxf")
```

---

## Common mistakes and pitfalls

!!! warning "Editable fields require a compatible template"
    Fill Template Fields only shows fields that have `freecad:editable`
    attributes in the SVG. A plain SVG with no editable attributes shows an
    empty dialog. Use one of the bundled FreeCAD templates or add
    `freecad:editable` attributes to your custom template in Inkscape.

!!! warning "Switching templates removes editable field values"
    If you change the template of an existing page, any previously filled
    field values are discarded because the new template may have different
    field names. Re-fill the fields after switching templates.

!!! warning "SVG export does not round-trip back to FreeCAD"
    The exported SVG is a static snapshot. Opening it in FreeCAD does not
    restore the parametric TechDraw document — it shows only the artwork.
    Keep the `.FCStd` file as the master and export SVG/DXF as needed.

!!! warning "DXF text encoding"
    DXF files use a code-page encoding that may not support all Unicode
    characters. Special symbols (°, ∅, ±) may need to be entered using DXF
    control codes (`%%d`, `%%c`, `%%p`) if the downstream CAD tool does not
    support Unicode DXF.

---

## See also

- Views — project 3-D geometry onto the page
- Dimensions — add linked measurements to views
- Fill Template Fields — fill in the title block text
