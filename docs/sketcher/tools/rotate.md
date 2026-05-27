# Rotate / Polar Transform

> **In one sentence:** Rotate selected sketch geometry about a centre point, or
> create a polar (circular) array of copies evenly distributed around that centre.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher tools → Rotate / Polar Transform  
**Toolbar:** Sketcher Tools · **Shortcut:** none

The Rotate tool moves geometry through an angle about a specified centre point.
With more than one copy specified, it creates a polar array — copies placed at
equal angular intervals around the centre, like spokes on a wheel.

---

## Intuition

Think of a clock face: if you draw one clock hand and use Rotate with 12 copies
and 30° per step, you get a full set of hour markers evenly spaced around the
centre. Rotate / Polar Transform is that circular repetition tool.

---

## When to use it

- Placing bolt holes evenly around a bolt circle (polar array).
- Rotating a cam profile or tooth to a specific angular position.
- Creating a fan blade or radial fin pattern.
- Rotating one sketch element to align it to a non-axis angle.

## When NOT to use it

- **You need a rectangular grid** — use [Move / Array Transform](translate.md).
- **You need a mirror copy** — use [Mirror](symmetry.md).

---

## Step-by-step walkthrough

1. **Select the geometry** to rotate or copy.
2. **Activate Rotate / Polar Transform** from the toolbar or menu.
3. **Click the rotation centre point** in the viewport.
4. In the task panel:
    - **Number of copies:** 0 for a pure rotation; N for N additional copies.
    - **Total angle:** the full angular span of the array, or just the rotation
      angle for a single rotation.
    - **Step angle:** angle between consecutive copies (= total angle / N).
5. **Click OK.**

!!! tip
    For a full circle array, set **Total angle = 360°** and **Number of copies** to
    the number of features − 1 (the original counts as 1, so for 6 holes set
    copies = 5).

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Number of copies | Integer ≥ 0 | 0 | Extra copies. 0 = rotate geometry in place. |
| Total angle | Degrees | 360° | Angular span of the full array. |
| Step angle | Degrees | Computed | Angle between consecutive copies. |
| Centre point | 2-D coordinate | Interactive click | The pivot of the rotation. |
| Clone | Toggle | Off | If on, copies are Clones with constraints linked to original. |

---

## Common mistakes and pitfalls

!!! warning "Polar array does not close evenly"
    **Cause:** The total angle (360°) is not evenly divisible by the step angle,
    leaving a gap before the last copy re-meets the first.  
    **Fix:** Set the total angle to exactly 360° and let the step angle be computed
    as 360° / (copies + 1).

!!! warning "Centre point snaps to wrong location"
    **Cause:** The centre point click landed on a nearby vertex rather than the
    intended centre.  
    **Fix:** Place a [Point](create-point.md) at the intended centre and use
    [Lock Position](constraint-lock.md) to fix it before running Rotate.

---

## Python API

```python
# Rotate is GUI-only; the underlying method may be:
# sketch.addPolarArray(geoIds, centre, angle, copies, clone)
# TODO: verify against Sketcher source.
print("Use the Rotate / Polar Transform tool interactively.")
```

---

## See also

- [Move / Array Transform](translate.md) — rectangular array
- [Symmetry (Mirror)](symmetry.md) — mirror about a line
- [Copy / Clone / Move](copy-clone-move.md) — non-array single-copy operations
- [Angle](constraint-angle.md) — set angular positions after rotation
