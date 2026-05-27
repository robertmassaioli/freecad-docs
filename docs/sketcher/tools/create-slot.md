# Slot / Arc Slot

> **In one sentence:** Draw a closed slotted profile — two parallel straight ends
> joined by two semicircles (Slot) or a profile bounded by two concentric arcs
> (Arc Slot) — in a single operation.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Slot  
**Toolbar:** Sketcher Geometries (Slot dropdown) · **Shortcut:** `G, S` (Slot) / `G, A, S` (Arc Slot)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Slot | `G, S` | Straight slot: two semicircles joined by two parallel lines |
| Arc Slot | `G, A, S` | Curved slot: two concentric arcs joined by two radial lines |

---

## Intuition

A slot is what you get when you drag a circle along a straight path — the circle
sweeps out a "stadium" shape with flat sides and rounded ends. Slot creates this
shape in one step.

An Arc Slot is the curved version: imagine dragging a circle along an arc instead
of a straight line. The result is a profile bounded by two concentric arcs and two
radial end-walls — the shape of a curved slot in a flanged plate or a radial
adjustment slot on a mounting bracket.

---

## When to use it

- You need an elongated hole that allows a bolt to slide (straight slot).
- You need an adjustment slot that follows a bolt circle pattern (arc slot).
- You are modelling a track, guide channel, or oblong pocket.

## When NOT to use it

- **You need an oblong shape that is not aligned to an axis** — Slot is easiest
  when oriented naturally. For an angled slot, use Slot then rotate/mirror it, or
  draw manually with lines and arcs.
- **You need the slot to be a different shape at each end** — draw individual lines
  and arcs and constrain them manually.

---

## Step-by-step walkthrough

### Straight Slot

1. **Press `G, S`** or select **Slot** from the dropdown.
2. **Click the centre of the first semicircle** (one end of the slot).
3. **Click the centre of the second semicircle** (the other end). The slot appears
   spanning between the two centres with a width equal to twice the default radius.
4. **Constrain the slot:**
    - Set the end-cap radius with [Radius/Diameter](constraint-radius-diameter.md).
    - Set the centre-to-centre length with [Dimension](dimension.md) or
      [Distance](constraint-distance.md).
    - Fix the orientation with [Horizontal](constraint-horizontal-vertical.md) or
      [Angle](constraint-angle.md).
    - Fix the position with [Lock Position](constraint-lock.md) or
      [Coincident](constraint-coincident.md).

### Arc Slot

1. **Press `G, A, S`** or select **Arc Slot** from the dropdown.
2. **Click the arc centre** (the pivot of the curvature).
3. **Click the centre of the first end-cap.**
4. **Click the centre of the second end-cap.** The arc slot sweeps around the pivot.
5. **Constrain:**
    - Inner and outer radius (use [Radius/Diameter](constraint-radius-diameter.md)
      on the inner and outer arcs).
    - Angular span with [Angle](constraint-angle.md).
    - Pivot position.

---

## Parameters

### Slot

| Parameter | Type | Applied via |
|-----------|------|-------------|
| End-cap radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) on either semicircle |
| Centre-to-centre length | Distance | [Distance](constraint-distance.md) between the two semicircle centres |
| Orientation | Angle | [Horizontal/Vertical](constraint-horizontal-vertical.md) or [Angle](constraint-angle.md) |
| Position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) |

### Arc Slot

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Inner radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) on the inner arc |
| Outer radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) on the outer arc |
| Angular span | Angle | [Angle](constraint-angle.md) on the bounding radii |
| Pivot position | Coordinate | [Lock Position](constraint-lock.md) on the pivot point |

---

## Common mistakes and pitfalls

!!! warning "Slot not fully constrained after setting radius and length"
    **Cause:** The slot still has a rotational degree of freedom (it can pivot about
    its own centreline) and a positional degree of freedom (it can translate).  
    **Fix:** Add a [Horizontal](constraint-horizontal-vertical.md) or
    [Angle](constraint-angle.md) constraint to fix orientation, and a
    [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md)
    to fix position.

!!! warning "Arc slot inner and outer radii are equal"
    **Cause:** The third click was not far enough from the second, resulting in a
    zero-width slot.  
    **Fix:** Delete and redraw with a greater separation between the inner and outer
    arcs.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# A horizontal slot: two arcs + two lines
# This is the low-level equivalent of what the Slot tool creates.
# For simplicity, use the GUI tool and constrain programmatically.

# Fix a slot's semicircle radius
# (geo index of first arc is idx_arc0, etc.)
# sketch.addConstraint(Sketcher.Constraint("Radius", idx_arc0, 5.0))
# TODO: verify — the exact GeoId layout produced by the Slot tool
print("Use the Slot tool interactively; constrain via Python after creation.")
```

---

## See also

- [Create Rectangle](create-rectangle.md) — rectangular profiles
- [Create Arc](create-arc.md) — individual arc segments
- [Radius/Diameter](constraint-radius-diameter.md) — constrain slot end-cap radius
- [Distance](constraint-distance.md) — set slot length
