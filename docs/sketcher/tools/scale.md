# Scale

> **In one sentence:** Scale selected sketch geometry by a numeric factor about
> a specified centre point, shrinking or enlarging it proportionally.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher tools → Scale  
**Toolbar:** Sketcher Tools · **Shortcut:** none

Scale multiplies all coordinate offsets from the centre point by the specified
factor. A factor > 1 enlarges; 0 < factor < 1 shrinks; a negative factor also
mirrors. All constraints that reference the scaled geometry are preserved.

---

## Intuition

Think of a photocopier with a zoom setting: you feed in the original and get back
a version that is 150% or 75% of the original size, centred on the same point.
Scale does the same thing to sketch geometry.

---

## When to use it

- You drew a profile in approximate size and want to rescale it to the correct
  size without re-drawing.
- You are making a scaled copy of a reference profile.
- You want to proportionally shrink a complex outline.

## When NOT to use it

- **You want to resize one dimension without the other** — use
  [Distance](constraint-distance.md) constraints to individually control width
  and height.
- **You want uniform sizing from the start** — apply dimensional constraints as
  you draw; scaling after the fact is rarely the clearest parametric approach.

---

## Step-by-step walkthrough

1. **Select the geometry** to scale.
2. **Activate Scale** from the toolbar or menu.
3. **Click the centre point** of the scaling operation (the fixed point that does
   not move).
4. **Enter the scale factor** in the task panel (e.g. 2.0 = double size,
   0.5 = half size).
5. **Click OK.**

!!! tip
    If you want the geometry to remain at the sketch origin after scaling, click the
    origin point (0, 0) as the centre before entering the factor.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Scale factor | Float | 1.0 | Multiplier applied to all geometry distances from the centre point. |
| Centre point | 2-D coordinate | Interactive | The fixed point about which scaling is applied. |
| Copy | Toggle | Off | If on, the original geometry is preserved and a scaled copy is created. |

---

## Common mistakes and pitfalls

!!! warning "Constraints become redundant after scaling"
    **Cause:** Dimensional constraints (lengths, radii) are set to specific values;
    after scaling the actual geometry changes but old constraint values may conflict.  
    **Fix:** After scaling, update the dimensional constraints to the new expected
    values, or set them to Reference mode before scaling and re-add driving
    constraints afterward.

---

## Python API

```python
# Scale is GUI-only in FreeCAD 1.1.
# TODO: verify whether SketchObject exposes a scale() method.
print("Use the Scale tool interactively.")
```

---

## See also

- [Move / Array Transform](translate.md) — move without scaling
- [Rotate / Polar Transform](rotate.md) — rotate without scaling
- [Distance](constraint-distance.md) — set specific dimensions after scaling
