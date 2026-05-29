# Customise Dimension Format

> **In one sentence:** Opens a dialog to set the printf-style format string
> for selected dimensions — controlling decimal places, prefixes, and suffixes.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Attributes/Modifications → Customise Dimension Format

The format string uses printf-style notation applied to the `FormatSpec`
dimension property.

---

## Format string reference

| Format | Output |
|--------|--------|
| `%.0f` | No decimal places (integer) |
| `%.1f` | One decimal place |
| `%.2f` | Two decimal places (default) |
| `%.3f` | Three decimal places |
| `%g` | Significant figures (removes trailing zeros) |

### Adding prefix and suffix

| FormatSpec | Output example |
|------------|----------------|
| `"⌀%.2f"` | ⌀25.00 |
| `"R%.1f"` | R12.5 |
| `"%.2f ±0.05"` | 25.00 ±0.05 |

---

## See also

- [Prefix Tools](prefix-tools.md) — quick prefix insertion via dedicated buttons
- [Decimal Places](decimal-places.md) — bulk increase/decrease decimal places
- [TechDraw Workbench](../../index.md) — workbench overview
