# Drilling

> **In one sentence:** Generate drilling, boring, and peck-drilling cycles
> for circular holes detected from the model geometry.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Drilling  
**Command:** `CAM_Drilling`  
**Shortcut:** none

Generates canned drilling cycles (G81, G82, G83, G85) for circular holes in
the model. FreeCAD detects circular edges or cylindrical faces and creates a
drill cycle at each centre point.

---

## Intuition

Drilling is different from milling — it uses a point tool (drill bit) that
plunges straight down. FreeCAD's Drilling operation detects all the hole
centres in the model and generates the appropriate canned cycle for each,
in the selected depth mode.

---

## When to use it

- Drilling through-holes or blind holes in a part.
- Boring existing holes for size and finish.
- Peck drilling deep holes where chip evacuation is needed.

---

## Cycle types

| Cycle | G-code | Description |
|-------|--------|-------------|
| G81 | G81 | Simple drill — plunge to depth |
| G82 | G82 | Drill with dwell at bottom |
| G83 | G83 | Peck drilling — retract fully between pecks |
| G85 | G85 | Boring — feed in, feed out |

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Drill Mode | Cycle type (G81, G82, G83, G85) |
| Peck Depth | Depth of each peck in G83 mode |
| Dwell Time | Pause at bottom in G82 mode (seconds) |
| Extra Offset | Additional depth beyond the hole bottom |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## Common mistakes and pitfalls

!!! warning "Drilling misses some holes"
    FreeCAD only detects holes from complete 360° circular edges or cylindrical
    faces. Partial arcs or holes from mesh models are not detected. Use the
    **Base Geometry** panel to manually add missed hole centres.

---

## See also

- [Helix](helix.md) — interpolate circular features with an end mill
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
