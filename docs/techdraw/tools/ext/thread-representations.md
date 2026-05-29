# Thread Representations

> **In one sentence:** Draws ISO 6410 / ASME Y14.6 simplified thread
> representations (solid crest, dashed root lines) on threaded holes and bolts.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Centerlines/Threading

Four tools are available, one for each combination of hole/bolt × side/bottom:

| Tool | Menu name | What it draws |
|------|-----------|---------------|
| Thread Hole (Side) | Draw Thread Hole (Side View) | Inner thread: solid minor diameter, dashed major diameter |
| Thread Hole (Bottom) | Draw Thread Hole (Bottom View) | Inner thread: full minor circle, ¾ major circle |
| Thread Bolt (Side) | Draw Thread Bolt (Side View) | External thread: solid major diameter, dashed minor diameter |
| Thread Bolt (Bottom) | Draw Thread Bolt (Bottom View) | External thread: full major circle, ¾ minor circle |

---

## Step-by-step (all thread tools)

1. Click the circular edge of the hole or bolt in the view.
2. Choose the appropriate thread representation tool.
3. The cosmetic lines appear in the correct ISO style.
4. Use Insert Diameter Dimension to dimension the thread nominal diameter,
   then manually set `FormatSpec` to add the thread designation
   (e.g. `"M12 × 1.75"`).

---

## Common mistakes and pitfalls

!!! warning "Thread tools draw cosmetic lines — not geometry"
    The thread representation tools draw ISO-conventional cosmetic lines. They
    do not create thread geometry in the 3-D model and do not affect hole or
    shaft dimensions. You still need the correct nominal dimension and thread
    designation text.

---

## See also

- [Add Circle Centerlines](circle-centerlines.md) — centerlines for hole circles
- [Insert Diameter Dimension](../dim-diameter.md) — dimension the nominal thread diameter
- [TechDraw Workbench](../../index.md) — workbench overview
