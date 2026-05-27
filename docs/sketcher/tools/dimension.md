# Dimension (Unified)

> **In one sentence:** A smart, context-sensitive constraint tool that
> automatically detects whether to apply a distance, radius, diameter, or angle
> constraint based on what you have selected — and lets you cycle through types
> with the `M` key.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Dimension  
**Toolbar:** Sketcher Constraints · **Shortcut:** `D`

The unified Dimension tool (`Sketcher_Dimension`) was introduced in FreeCAD 1.0
as the recommended way to add dimensional constraints. It replaces the need to
remember separate shortcuts for Distance, Radius, Angle, etc. by detecting the
appropriate type from the current selection and offering a one-click confirmation
or a `M`-key cycle to switch between available types.

**Driving vs Reference mode:** Every dimensional constraint, including those
created by the unified Dimension tool, can be toggled between *driving* (controls
the geometry) and *reference* (reads but does not constrain). See the deep dive
below.

---

## Intuition

Think of Dimension as a universal measuring tape that also *sets* the measurement:
select what you want to measure, press `D`, and the tool figures out whether you
are measuring a length, a radius, or an angle. If it picks the wrong type, press
`M` to cycle to the next available type for your selection.

The value dialog appears immediately with the current measurement pre-filled. Type
the desired value and confirm. The constraint is applied.

---

## When to use it

- Any time you need to set a numeric value on sketch geometry — this is now the
  primary constraint entry point.
- Select a single line → distance along the line.
- Select a circle or arc → radius (or cycle with `M` to diameter).
- Select two lines → angle between them.
- Select two points → point-to-point distance (or horizontal/vertical components
  with `M`).
- You want to toggle a dimension between driving and reference without using
  the dedicated toggle tool.

## When NOT to use it

- **You need a very specific constraint type and know the exact shortcut** —
  using the dedicated tool (`K, R` for Radius, `L` for Horizontal Distance, etc.)
  is faster for experienced users.
- **You need a collinear or parallel constraint** — those are geometric, not
  dimensional; use the dedicated geometric constraint tools.

---

## Step-by-step walkthrough

1. **Select geometry** — a line, circle, arc, two lines, two points, or a
   point and a line.
2. **Press `D`** or activate **Dimension** from the toolbar.
3. A **dimension preview** appears on the geometry. The tool auto-selects the
   most appropriate constraint type.
4. If the wrong type is shown, **press `M`** to cycle through available types
   for the current selection (e.g. straight distance → horizontal distance →
   vertical distance, or radius → diameter).
5. **Click to place** the dimension label at a convenient location.
6. A **value dialog** appears with the current measurement pre-filled.
   Edit the value if needed and click **OK** (or press ++enter++).

!!! tip
    You can leave the pre-filled value unchanged and just click OK — this sets the
    constraint to the *current* measurement, effectively locking it in place without
    changing the geometry.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Constraint value | Distance / Angle / Radius | Current measurement | The numeric value to enforce. |
| Driving / Reference | Toggle | Driving | Whether the constraint controls geometry or just reads its value. |

---

--8<-- "sketcher/driving-vs-reference.md"

---

## Dimension type auto-detection

The type the Dimension tool selects depends on your selection:

| Selection | Default type | `M` cycles through |
|-----------|-------------|-------------------|
| 1 line | Line length (distance) | Horizontal distance → Vertical distance |
| 1 circle | Radius | Diameter |
| 1 arc | Radius | Diameter |
| 2 points | Point-to-point distance | Horizontal distance → Vertical distance |
| 2 lines | Angle between them | — |
| 1 point + 1 line | Distance from point to line | — |
| 1 line + 1 arc | Angle at tangent point | — |

---

## Advanced usage

### Setting reference (inspection) dimensions

During step 6 (the value dialog), click the **Reference** radio button before
confirming. The dimension is shown in blue and does not constrain geometry.
This is useful for checking a derived length or angle without adding a constraint
that could conflict.

### Formula-driven dimensions

The value field accepts FreeCAD formula expressions. Type `=Spreadsheet.Width`
to drive the dimension from a spreadsheet cell, enabling full parametric design.

### Batch dimensioning

You can select multiple geometry elements before pressing `D`; Dimension will
apply a constraint to each selected element in sequence, prompting for a value
each time.

---

## Common mistakes and pitfalls

!!! warning "Dimension shows in red (over-constrained)"
    **Cause:** The value you set conflicts with an existing constraint that already
    determines this measurement.  
    **Fix:** Delete one of the conflicting constraints. The Dimension tool highlights
    which are redundant.

!!! warning "Dimension cycled to the wrong type with M"
    **Cause:** You pressed `M` one too many times and went past the desired type.  
    **Fix:** Press `M` again to cycle back, or press ++esc++ and re-apply from
    scratch.

!!! warning "Reference dimension shows 0 or a wrong value"
    **Cause:** The geometry has not been solved to a final position yet (the sketch
    is under-constrained), so the reference dimension reads the current floating
    position.  
    **Fix:** Fully constrain the sketch first, then add reference dimensions to
    read derived values.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Add a line
idx = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(30, 0, 0)), False)

# Apply a driving distance constraint (equivalent to Dimension → distance)
sketch.addConstraint(Sketcher.Constraint("Distance", idx, 30.0))

# Apply a reference distance constraint (blue, non-driving)
sketch.addConstraint(Sketcher.Constraint("Distance", idx, 30.0))
# Set the last constraint to reference mode:
last = len(sketch.Constraints) - 1
sketch.setDriving(last, False)

doc.recompute()
```

### Key method

```python
sketch.setDriving(constraintIndex, isDriving)
# isDriving=True  → driving (controls geometry)
# isDriving=False → reference (reads geometry)
```

---

## See also

- [Distance](constraint-distance.md) — dedicated distance constraint
- [Radius/Diameter](constraint-radius-diameter.md) — dedicated radius/diameter constraint
- [Angle](constraint-angle.md) — dedicated angle constraint
- [Toggle Driving / Active](toggle-constraint.md) — flip an existing constraint between driving and reference
- [Lock Position](constraint-lock.md) — absolute X, Y coordinate constraints
