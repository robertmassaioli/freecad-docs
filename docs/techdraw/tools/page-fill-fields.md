# Fill Template Fields

> **In one sentence:** Opens a dialog to type values into the editable text
> fields defined in the page template — for filling in the title block.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page → Fill Template Fields

Every TechDraw template can define named editable text fields — title, scale,
drawn-by, date, revision, and so on. Fill Template Fields shows these fields
in a dialog so you can fill them in without opening an SVG editor.

---

## Step-by-step

1. Select the page in the model tree (or make a drawing page active).
2. Choose **TechDraw → Page → Fill Template Fields**.
3. A dialog lists all editable field names found in the template SVG.
4. Type the value for each field.
5. Click **OK**. The title block text updates instantly on the page.

---

## Common standard fields (ISO 7200)

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
that are actually present in the current template's SVG.

---

## Python API

```python
page = doc.getObject("Page")
template = page.Template

# List all editable fields
fields = template.EditableTexts   # returns a dict {fieldName: currentValue}

# Set values
fields["FC-Title"] = "Bracket Assembly"
fields["FC-Author"] = "R. Massaioli"
fields["FC-Date"] = "2026-05-29"
template.EditableTexts = fields
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Editable fields require a compatible template"
    Fill Template Fields only shows fields that have `freecad:editable`
    attributes in the template SVG. A plain SVG with no editable attributes
    shows an empty dialog. Use one of the bundled FreeCAD templates or add
    `freecad:editable` attributes to your custom template in Inkscape.

!!! warning "Switching templates removes editable field values"
    If you change the template of an existing page, any previously filled
    field values are discarded because the new template may have different
    field names. Re-fill the fields after switching templates.

---

## See also

- [Insert Default Page](page-insert-default.md) — create a page with the default template
- [Insert Page Using Template](page-insert-template.md) — create a page with a chosen template
- [TechDraw Workbench](../index.md) — workbench overview
