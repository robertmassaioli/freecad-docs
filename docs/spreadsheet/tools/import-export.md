# Import / Export

> **In one sentence:** Load an existing CSV file into a new FreeCAD spreadsheet
> (Import), or save the current spreadsheet's data as a CSV file for use in
> external applications (Export).

---

## Overview

**Workbench:** Spreadsheet  
**Menu:** Spreadsheet → Import Spreadsheet / Export Spreadsheet  
**Toolbar:** Spreadsheet · **Shortcut:** none

| Tool | Description |
|------|-------------|
| [Import](#import) | Read a CSV file and populate a new spreadsheet object |
| [Export](#export) | Write the current spreadsheet to a CSV file |

---

## Intuition

FreeCAD's native spreadsheet is embedded in the `.FCStd` file and is not
directly editable by Excel or LibreOffice. Import and Export bridge that gap:

- **Import** brings in data you computed outside FreeCAD (material databases,
  supplier tables, engineering lookup tables) so formulas and aliases can
  reference them.
- **Export** lets you take computed results (masses, areas, bill-of-materials
  rows) out to a spreadsheet program, a reporting tool, or another document.

Neither operation is round-trip: Import replaces cell content from the file;
Export writes current display values (not formulas) to the file. Aliases and
formulas are not preserved in CSV format.

---

## Import

**Menu:** Spreadsheet → Import Spreadsheet

### What it does

Opens a file browser. Select a `.csv` file. FreeCAD creates a **new**
`Spreadsheet::Sheet` object and populates it with the CSV data. Existing
spreadsheets in the document are not modified.

### CSV format requirements

FreeCAD's CSV importer is strict:

| Requirement | Detail |
|-------------|--------|
| Delimiter | Comma (`,`) by default. Semicolon (`;`) may be supported depending on locale. |
| Encoding | UTF-8 recommended |
| Quoting | Standard CSV quoting (`"text with, comma"`) |
| Header row | Optional — FreeCAD treats row 1 as data, not special header |
| Numbers | Use the decimal point (`.`), not the decimal comma (`,`) |

### After importing

- Cell content is imported as text or numbers, depending on the CSV value type.
- **No aliases are set** — set them manually after import.
- **No formulas** — formulas in a CSV file are stored as text strings starting
  with `=`. FreeCAD evaluates them if they use valid expression syntax.

### Step-by-step

1. **Spreadsheet → Import Spreadsheet.**
2. Navigate to the `.csv` file.
3. Click **Open**.
4. A new spreadsheet named after the file appears in the model tree.
5. Open it, verify the data, and set aliases as needed.

---

## Export

**Menu:** Spreadsheet → Export Spreadsheet

### What it does

Opens a Save dialog. The current spreadsheet's **display values** are written
to a CSV file. Formulas are evaluated; only the resulting values are exported.

### What is exported (and what is not)

| Exported | Not exported |
|---------|-------------|
| Cell values (computed) | Formula text |
| Text content | Aliases |
| Number values | Colour formatting |
| Empty cells (as empty fields) | Merged cell spans |

### Step-by-step

1. **Click the spreadsheet** in the model tree to make it active.
2. **Spreadsheet → Export Spreadsheet.**
3. Choose the destination file and name.
4. Click **Save**.

---

## Python API

### Import

```python
import FreeCAD as App
import Spreadsheet

doc = App.newDocument()

# Import a CSV into a new spreadsheet
# This is typically done via the GUI; the equivalent Python:
sheet = doc.addObject("Spreadsheet::Sheet", "Imported")

# Manually populate from CSV (since there's no direct Python import API)
import csv

with open("/path/to/data.csv", newline="", encoding="utf-8") as f:
    reader = csv.reader(f)
    for row_idx, row in enumerate(reader, start=1):
        for col_idx, value in enumerate(row):
            # Convert column index to letter (0→A, 1→B, …)
            col_letter = chr(ord("A") + col_idx)
            cell_addr = f"{col_letter}{row_idx}"
            sheet.set(cell_addr, value)

doc.recompute()
```

### Export

```python
import FreeCAD as App
import csv

doc = App.activeDocument()
sheet = doc.getObject("Spreadsheet")

# Find the used range
rows = 20   # adjust to your data
cols = 5    # adjust to your data

with open("/path/to/output.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    for r in range(1, rows + 1):
        row_data = []
        for c in range(cols):
            col_letter = chr(ord("A") + c)
            cell = f"{col_letter}{r}"
            try:
                row_data.append(str(sheet.get(cell)))
            except Exception:
                row_data.append("")
        writer.writerow(row_data)

print("Export complete.")
```

---

## Common mistakes and pitfalls

!!! warning "Import creates a new sheet — it does not update an existing one"
    Each import creates a fresh spreadsheet object. If you want to update data
    in an existing spreadsheet from a new CSV, you must either delete and
    re-import, or update cells manually / via Python.

!!! warning "Formulas exported as values"
    The exported CSV contains the *result* of each formula, not the formula
    text. If you re-import the CSV and expect FreeCAD to have formulas, they
    will not be there — only the values.

!!! warning "Locale decimal separator"
    If your system locale uses `,` as the decimal separator, CSV files may have
    numbers like `3,14` which FreeCAD may misparse as two separate fields.
    Ensure your CSV uses `.` as the decimal point.

---

## See also

- [Create Spreadsheet](create-spreadsheet.md) — starting a spreadsheet from scratch
- [Aliases](aliases.md) — setting aliases after import
- [Cell Formatting](cell-formatting.md) — formatting the imported data for readability
