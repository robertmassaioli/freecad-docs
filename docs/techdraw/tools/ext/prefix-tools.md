# Prefix Tools

> **In one sentence:** Inserts or removes a prefix character (⌀, □, n×) on
> selected dimensions by editing their FormatSpec property.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Format/Organize Dimensions

Four tools:

| Tool | Effect | Description |
|------|--------|-------------|
| Insert Diameter Prefix | Prepends ⌀ | Add the diameter symbol |
| Insert Square Prefix | Prepends □ | Add the square cross-section symbol |
| Insert Repetition Prefix | Prepends n× | Add a count multiplier |
| Remove Prefix Character | Removes first char | Strip any single-character prefix |

---

## Step-by-step

1. Select one or more dimensions on the page.
2. Choose the desired prefix tool.
3. The prefix is prepended to (or removed from) the dimension text immediately.

---

## Common mistakes and pitfalls

!!! warning "Multiple prefixes stack"
    Applying multiple prefix tools in sequence prepends both prefixes
    (e.g. `⌀R%.2f`). Check the dimension text after applying prefixes.

---

## See also

- [Customise Dimension Format](customise-dimension-format.md) — full FormatSpec editing
- [Decimal Places](decimal-places.md) — bulk decimal precision control
- [TechDraw Workbench](../../index.md) — workbench overview
