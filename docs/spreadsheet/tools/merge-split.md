# Merge / Split Cells

> **In one sentence:** Combine adjacent cells into a single wider or taller
> cell (Merge), or restore a merged cell back to individual cells (Split).

---

## Overview

**Workbench:** Spreadsheet  
**Menu:** Spreadsheet → Merge Cells / Split Cell  
**Toolbar:** Spreadsheet · **Shortcut:** none

| Tool | Description |
|------|-------------|
| [Merge Cells](#merge-cells) | Combine the selected rectangular range into one spanning cell |
| [Split Cell](#split-cell) | Restore a merged cell back to its constituent individual cells |

---

## Intuition

Merging cells is a **display and layout** operation, not a data operation. The
merged cell spans multiple columns or rows visually, making it useful for:

- **Section headings** in a spreadsheet that organise parameters into groups
  (e.g. "Structural Parameters" spanning columns A–D).
- **Wide label cells** when a description is too long for a single column.
- **Visual grouping** to make a complex spreadsheet easier to scan.

Merging does **not** combine data from multiple cells. Only the content of the
top-left cell of the selection is kept; all other cells in the merge range lose
their content.

---

## Merge Cells

**Menu:** Spreadsheet → Merge Cells

### Step-by-step

1. **Select the rectangular range** to merge (click and drag, or click the
   first cell and Shift+click the last).
2. **Spreadsheet → Merge Cells.**
3. The selected range becomes a single cell spanning all the selected columns
   and rows.
4. The content of the top-left cell of the range is kept. Content in the
   other cells is discarded.

### Rules

- The selection must be a rectangle (no non-contiguous selections).
- Cells within the range that have aliases retain the alias if they are the
  top-left cell; other cells' aliases are lost.
- A merged cell can still have an alias set on it.
- A merged cell's content and formulas work the same as a regular cell.

---

## Split Cell

**Menu:** Spreadsheet → Split Cell

### Step-by-step

1. **Click the merged cell** you want to split.
2. **Spreadsheet → Split Cell.**
3. The merged cell is split back into its constituent individual cells.
4. The content of the merged cell moves to the top-left cell of the split
   range. The other cells are empty.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
sheet = doc.addObject("Spreadsheet::Sheet", "Sheet")

# Set some content
sheet.set("A1", "Section Header")
sheet.set("A2", "Length")
sheet.set("B2", "200 mm")
doc.recompute()

# Merge A1:D1 to create a wide header
# (Use the GUI tool; programmatic merge via Python):
sheet.mergeCells("A1:D1")
doc.recompute()

# Split the merge back
sheet.splitCell("A1")
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Merging discards content of non-top-left cells"
    Select the range only after moving all important content to the top-left
    cell of the intended merge. There is no undo path for the discarded content
    other than Ctrl+Z immediately after.

!!! warning "Merged cells and formula ranges"
    Range functions like `sum(A1:A10)` treat a merged cell spanning A1:A3 as
    a single value at A1. The cells A2 and A3 appear empty to the formula.
    Avoid merging cells inside ranges used by aggregate formulas.

---

## See also

- [Cell Formatting](cell-formatting.md) — alignment, style, and colour for cells
- [Create Spreadsheet](create-spreadsheet.md) — spreadsheet basics
