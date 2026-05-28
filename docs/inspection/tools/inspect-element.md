# Inspect Element

> **In one sentence:** Inspect Element switches the cursor to a pipette and,
> as you hover or click over an `Inspection::Feature` result in the 3-D view,
> shows the signed deviation distance at that point.

---

## Overview

**Workbench:** Inspection  
**Menu:** Inspection → Inspection…  
**Command:** `Inspection_InspectElement`

Inspect Element is a point-by-point read-out tool. After running
[Visual Inspection](visual-inspection.md) to create an `Inspection::Feature`,
use Inspect Element to interactively hover over the coloured surface and read
the stored deviation value at any individual point.

The tool is only available (enabled) when the active document contains at
least one `Inspection::Feature`.

---

## Step-by-step

1. Run [Visual Inspection](visual-inspection.md) first to create an
   `Inspection::Feature` with computed distances.
2. Go to **Inspection → Inspection…**.
3. The cursor changes to a pipette icon.
4. Move the pipette over the coloured inspection result in the 3-D view.
   The distance value at the nearest stored point is shown in a tooltip or
   in the status bar at the bottom of the screen.
5. Click to fix the reading.
6. Press **Escape** or click **Inspection → Inspection…** again to exit
   pipette mode and restore the normal cursor.

---

## Reading the distance value

The displayed value is the **signed distance** stored in `Feature.Distances`
at the closest sampled point to where you clicked or hovered:

| Value | Meaning |
|-------|---------|
| Positive | Actual is outside the nominal — excess material |
| Negative | Actual is inside the nominal — missing material |
| Zero (≈ 0) | Actual coincides with the nominal surface |
| Very large / grey | Out of Search Radius — no valid measurement |

---

## Availability

The command is **greyed out** (inactive) unless:

- The active document contains at least one `Inspection::Feature`.
- The active view is a 3-D view that is not already in an editing state.

If the menu entry is greyed out, run [Visual Inspection](visual-inspection.md)
first to generate an `Inspection::Feature`.

---

## Common mistakes and pitfalls

!!! warning "Command is greyed out"
    **Cause:** No `Inspection::Feature` exists in the document.  
    **Fix:** Run **Inspection → Visual Inspection…** first to create one.

!!! warning "Clicking outside the inspection result does nothing"
    **Cause:** The pipette only reads points that belong to the
    `Inspection::Feature`. Clicking on other geometry or empty space yields
    no result.  
    **Fix:** Aim the pipette at the coloured surface of the inspection result.

---

## See also

- [Inspection Workbench](../index.md) — overview and colour bar explanation
- [Visual Inspection](visual-inspection.md) — create the Inspection::Feature first
