# Fillet / Chamfer

> **In one sentence:** Round two lines into a smooth arc at their meeting point
> (Fillet) or clip the corner with a straight cut (Chamfer), in either case
> adjusting the lines automatically to preserve the overall shape.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Fillet / Chamfer  
**Toolbar:** Sketcher Geometries (Fillet dropdown) · **Shortcut:** none

| Tool | Description |
|------|-------------|
| Fillet | Replace a sharp corner with a circular arc of a specified radius |
| Chamfer | Replace a sharp corner with a straight line at 45° (or custom angle) |

Both tools operate on an existing corner (the intersection of two lines). After
applying them, the two lines are trimmed and the new transition element (arc or
line) is inserted and constrained.

---

## Intuition

When you draw two lines that meet at a corner, the corner is sharp. Fillet
"rounds it off" by inserting a small arc, like the rounded corner of a piece of
sheet metal bent gently rather than cracked at a sharp fold. Chamfer "cuts the
corner" with a straight diagonal line, like the bevelled edge of a machine part.

Both tools trim the original lines back to where the arc or chamfer line begins,
then add the transition element with tangency (Fillet) or endpoint (Chamfer)
constraints.

---

## When to use it

- **Fillet:** Sheet metal forms, injection-moulded parts, any profile where a
  sharp corner would create a stress concentration.
- **Chamfer:** Machined parts where a small angled flat is the standard transition
  (bolt heads, shaft undercuts).
- You want to add the transition in 2-D sketch space rather than using the Part
  Design Fillet or Chamfer dress-up (which works on 3-D edges); the sketch-level
  tools are better when the profile is the primary design element.

## When NOT to use it

- **You need to round a 3-D solid edge** — use
  [Part Design Fillet](../../part-design/tools/fillet.md) or
  [Part Design Chamfer](../../part-design/tools/chamfer.md) on the solid instead.
- **You need a fillet between two arcs** — the Fillet tool works on line-to-line
  corners; arc-to-arc transitions require manual arc insertion with tangent
  constraints.

---

## Step-by-step walkthrough

### Fillet

1. **Enter sketch edit mode** with the sketch containing the corner you want to
   round.
2. **Activate Fillet** from the Fillet dropdown in the toolbar or menu.
3. **Click the corner point** (the vertex where the two lines meet). A circular
   arc appears, trimming the two lines.
4. **Set the radius:**
    - In the task panel input field that appears, type the desired radius and press
      ++enter++, or
    - Apply a [Radius/Diameter](constraint-radius-diameter.md) constraint to the arc
      after creation.
5. **Repeat** for other corners, then press ++esc++ to exit the tool.

### Chamfer

1. **Enter sketch edit mode.**
2. **Activate Chamfer** from the Fillet dropdown.
3. **Click the corner point.** A short diagonal line appears.
4. **Set the chamfer size:**
    - Type the distance value in the task panel input, or
    - Apply [Distance](constraint-distance.md) constraints to the chamfer line
      endpoints after creation.

!!! tip
    To produce a 45° chamfer of a specific size, add a
    [Horizontal Distance](constraint-distance.md#horizontal-distance) constraint to
    one endpoint of the chamfer line and an [Equal](constraint-equal.md) constraint
    to force the X and Y offsets to be equal.

---

## Parameters

### Fillet

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) on the resulting arc |

### Chamfer

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Chamfer leg length | Distance | [Distance](constraint-distance.md) on either endpoint |

---

## Common mistakes and pitfalls

!!! warning "Fillet fails with 'Not enough space for fillet'"
    **Cause:** The requested radius is larger than the shorter of the two lines.
    There is not enough room to trim both lines and still have geometry remaining.  
    **Fix:** Use a smaller radius, or extend the lines before filleting.

!!! warning "Fillet arc is not tangent to both lines"
    **Cause:** The arc was inserted correctly but a manual constraint deleted or
    overrode the automatic tangent constraints.  
    **Fix:** Check the arc's constraints and re-add [Tangent](constraint-tangent.md)
    constraints between the arc and each adjacent line if they are missing.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Apply a fillet between geo 0 and geo 1, both lines meeting at point 2/1
# The fillet is applied at the shared vertex.
sketch.fillet(0, 2, 1, 1, 3.0, True, False)
# Args: geo1, vertex1, geo2, vertex2, radius, trim, createCorner
doc.recompute()
```

### Key method signature

```python
sketch.fillet(
    geo1,          # int — index of first line
    vertex1,       # int — vertex on first line (1=start, 2=end)
    geo2,          # int — index of second line
    vertex2,       # int — vertex on second line
    radius,        # float — fillet radius in mm
    trim,          # bool — trim original lines to fillet (True recommended)
    createCorner,  # bool — keep the original corner point (usually False)
)
```

---

## See also

- [Create Arc](create-arc.md) — manual arc insertion for non-corner fillets
- [Tangent](constraint-tangent.md) — enforce tangency between arc and line
- [Part Design Fillet](../../part-design/tools/fillet.md) — round 3-D solid edges
- [Part Design Chamfer](../../part-design/tools/chamfer.md) — chamfer 3-D solid edges
