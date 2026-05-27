# Cell Formatting

> **In one sentence:** Control how cells look — text alignment, bold / italic /
> underline style, foreground and background colour — without affecting cell
> values or formulas.

---

## Overview

**Workbench:** Spreadsheet  
**Menu:** Spreadsheet → Alignment / Styles  
**Toolbar:** Spreadsheet · **Shortcut:** none

Formatting tools are **display-only** — they never affect cell values, formulas,
or aliases. Formatting is stored in the document and survives save/reload.

| Tool group | Tools |
|-----------|-------|
| [Horizontal alignment](#horizontal-alignment) | Align Left, Align Center, Align Right |
| [Vertical alignment](#vertical-alignment) | Align Top, Align VCenter, Align Bottom |
| [Text style](#text-style) | Bold, Italic, Underline |
| [Colour](#colour) | Foreground (text) colour, Background colour |
| [Cell Properties dialog](#cell-properties-dialog) | All formatting in one dialog, plus display format |

---

## Intuition

FreeCAD's spreadsheet formatting is intentionally minimal — it supports the
basic presentational needs for an engineering parameter sheet without trying to
replicate the full formatting capabilities of a general-purpose spreadsheet
application.

The most practical uses:

- **Colour-code parameter categories** — blue background for inputs, grey for
  computed outputs, red for out-of-range warnings.
- **Bold section headers** — distinguish parameter group names from their values.
- **Right-align numeric columns** — aligning numbers right makes columns of
  different magnitudes easier to scan.

---

## Horizontal alignment

**Menu:** Spreadsheet → Alignment → Align Left / Center / Right

| Tool | Command | Behaviour |
|------|---------|-----------|
| Align Left | `Spreadsheet_AlignLeft` | Text and numbers align to the left edge of the cell |
| Align Center | `Spreadsheet_AlignCenter` | Content centred horizontally |
| Align Right | `Spreadsheet_AlignRight` | Text and numbers align to the right edge |

### Default alignment

- **Numbers and quantities:** right-aligned by default.
- **Text:** left-aligned by default.

Select one or more cells, then click the alignment button. The alignment
applies to all selected cells.

---

## Vertical alignment

**Menu:** Spreadsheet → Alignment → Align Top / VCenter / Bottom

| Tool | Command | Behaviour |
|------|---------|-----------|
| Align Top | `Spreadsheet_AlignTop` | Content at the top of the cell |
| Align VCenter | `Spreadsheet_AlignVCenter` | Content centred vertically |
| Align Bottom | `Spreadsheet_AlignBottom` | Content at the bottom of the cell |

Vertical alignment is most visible when row height is increased (by dragging
the row header border).

---

## Text style

**Menu:** Spreadsheet → Styles → Bold / Italic / Underline

| Tool | Command | Shortcut (in cell) | Behaviour |
|------|---------|-------------------|-----------|
| Bold | `Spreadsheet_StyleBold` | none | Toggle bold weight |
| Italic | `Spreadsheet_StyleItalic` | none | Toggle italic style |
| Underline | `Spreadsheet_StyleUnderline` | none | Toggle underline |

Styles are toggles: applying Bold to a bold cell removes the bold. Multiple
styles can be combined (bold + italic is valid).

---

## Colour

**Toolbar:** Foreground colour picker / Background colour picker (toolbar dropdowns)

The two colour pickers are in the Spreadsheet toolbar (not the menu):

| Picker | Effect |
|--------|--------|
| **Foreground colour** (text icon with coloured bar) | Sets the text colour of selected cells |
| **Background colour** (paint bucket icon) | Sets the cell background fill colour |

### Using the colour picker

1. **Select the cells** to colour.
2. Click the **dropdown arrow** on the colour picker button.
3. Click a colour from the palette, or click **Other…** for a custom colour.
4. The colour is applied immediately.

### Resetting to default

Click the colour picker dropdown → select the default swatch (usually
top-left, labelled "Default" or showing a dotted border).

---

## Cell Properties dialog

**Right-click a cell → Properties** (or select cells and right-click)

The Properties dialog is the most comprehensive formatting interface. It
combines all formatting options in one place and adds one extra feature:
**display format**.

### Tabs

#### Cell tab

| Field | Description |
|-------|-------------|
| Alias | The cell's alias name (see [Aliases](aliases.md)) |
| Display unit | Override the unit shown, without changing the stored value (e.g. store in mm, display in cm) |

#### Style tab

All text-style checkboxes (Bold, Italic, Underline) and colour pickers.

#### Alignment tab

Horizontal and vertical alignment selectors.

### Display unit

The Display Unit field is particularly useful for engineering spreadsheets:

- Store a length as `2540 mm` (internal storage is mm).
- Set Display Unit to `in` — the cell shows `100 in` instead of `2540 mm`.
- Expressions referencing this cell still get the raw value in mm.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
sheet = doc.addObject("Spreadsheet::Sheet", "Sheet")

# Set some values
sheet.set("A1", "Parameter")
sheet.set("B1", "Value")
sheet.set("A2", "Length")
sheet.set("B2", "200 mm")
sheet.set("A3", "Width")
sheet.set("B3", "80 mm")
doc.recompute()

# Bold the header row
sheet.setStyle("A1:B1", "bold")

# Right-align the value column
sheet.setAlignment("B1:B3", "right|vcenter")

# Set background colour for headers (R, G, B as floats 0.0–1.0)
sheet.setBackground("A1:B1", (0.7, 0.85, 1.0))   # light blue

# Set foreground (text) colour
sheet.setForeground("A2:A3", (0.3, 0.3, 0.3))     # dark grey labels

# Style string syntax:  "bold", "italic", "underline", combinations: "bold|italic"
sheet.setStyle("A2:A3", "italic")

doc.recompute()
```

### Alignment string syntax

| Token | Meaning |
|-------|---------|
| `left` | Align left |
| `right` | Align right |
| `center` | Align centre (horizontal) |
| `top` | Align top |
| `bottom` | Align bottom |
| `vcenter` | Align centre (vertical) |

Combine horizontal and vertical with `|`: `"right|vcenter"`.

---

## Common mistakes and pitfalls

!!! warning "Formatting does not export to CSV"
    When you export a spreadsheet to CSV, all formatting (colours, bold, alignment)
    is lost. Only the cell values are exported. If you need formatted output,
    use a LibreOffice Calc macro to apply formatting after importing the CSV.

!!! warning "Display unit does not change the stored value"
    Setting Display Unit to `in` on a cell containing `200 mm` shows `7.87 in`
    but the stored value is still 200 mm. Expressions referencing the cell get
    200 mm. This is a display-only override.

---

## See also

- [Merge / Split Cells](merge-split.md) — combine cells for layout purposes
- [Aliases](aliases.md) — set aliases via the Cell Properties dialog
- [Create Spreadsheet](create-spreadsheet.md) — spreadsheet basics
