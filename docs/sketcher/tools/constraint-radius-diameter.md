# Radius / Diameter Constraints

> **In one sentence:** Fix the size of a circle or arc by specifying its radius
> or diameter, in either driving or reference mode.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Radius / Diameter / Radius/Diameter  
**Toolbar:** Sketcher Constraints · **Shortcut:** `K, R` (Radius) · `K, O` (Diameter) · `R` (auto Radiam)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Radius | `K, R` | Set the radius of a circle or arc |
| Diameter | `K, O` | Set the diameter of a circle (shown as Ø) |
| Radius/Diameter (Radiam) | `R` | Automatically applies Radius to arcs and Diameter to full circles |

The **Radiam** tool (`R`) is the recommended default: it follows the drafting
convention that arcs are dimensioned by radius and full circles by diameter.

---

## Intuition

A circle has one degree of freedom for its size — you can shrink or grow it freely
until you specify its radius or diameter. These three constraints pin that single
DoF. Radius and Diameter are two ways of expressing the same measurement: `Ø = 2R`.

The Radiam tool removes the decision: for a full circle it shows a diameter (Ø)
label; for an arc it shows a radius (R) label. This matches standard engineering
drawing practice.

---

## When to use them

- Any time you draw a circle or arc and need to fix its size.
- Constraining the corner arcs of a Rounded Rectangle to a specific radius.
- Setting the slot end-cap radius.
- Ensuring all holes in a pattern have the same diameter (apply Radiam to one, then
  [Equal](constraint-equal.md) to the others).

## When NOT to use them

- **You need to constrain a B-Spline weight circle** — Radiam/Radius are not
  applicable to B-Spline knot circles; constrain the control point positions instead.
- **You want the circle's position** — Radius/Diameter only constrains size; use
  [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md)
  for position.

---

## Step-by-step walkthrough

1. **Select a circle or arc.**
2. **Press `R`** (Radiam) or `K, R` (Radius) or `K, O` (Diameter).
3. **Click to place** the dimension label (drag it away from the geometry for
   readability).
4. **Enter the value** in the dialog and click OK.

!!! tip
    After typing a value, press ++enter++ to confirm without clicking OK. This is
    faster for batch constraining multiple circles.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Value | Distance (mm) | Current radius | The radius (or diameter) to set. Must be positive. |
| Mode | Driving / Reference | Driving | See [Dimension → Driving vs Reference](dimension.md#driving-vs-reference-mode-in-depth). |

---

## Radius, Diameter, and Radiam in depth

All three tools write the same underlying constraint type (`Radius` in the solver)
but differ in how the value is interpreted and displayed:

#### Radius (`K, R`)

Displays an `R` label. The value entered is the radius directly.
Applied to both circles and arcs.

**Use Radius when:**
- You are dimensioning an arc (standard drafting practice).
- The design spec provides a radius value.

#### Diameter (`K, O`)

Displays an `Ø` label. The value entered is the diameter (2 × radius).
Applied to circles; also works on arcs but is unusual.

**Use Diameter when:**
- The design spec provides a diameter (common for holes, shafts, bosses).
- You need to match a nominal size from a standard (M6 bolt = Ø6 mm).

#### Radius/Diameter — Radiam (`R`)

The smart default: shows `R` for arcs, `Ø` for full circles.

**Use Radiam:**
- Always, unless you need to override the convention (e.g. you want a diameter
  label on an arc for some reason).

**Watch out for:** When cycling constraint types with the unified Dimension tool
(`D` + `M`), Radiam may flip between `R` and `Ø` display; check the label
before confirming to avoid entering the wrong scale.

---

## Common mistakes and pitfalls

!!! warning "Constraint applied but circle size does not change"
    **Cause:** The circle was already constrained to a different radius by an
    Equal constraint to another geometry. The new constraint conflicts.  
    **Fix:** Remove the Equal constraint or remove the old Radius constraint;
    keep only one that directly controls the size.

!!! warning "Diameter value entered when radius was expected"
    **Cause:** You applied Radius (`K, R`) but entered the diameter value,
    resulting in a circle twice the intended size.  
    **Fix:** Delete the constraint, reapply with the correct value, or switch to
    Diameter (`K, O`) and re-enter. Always check the label: `R 10` = radius 10 mm
    (diameter 20 mm); `Ø 10` = diameter 10 mm (radius 5 mm).

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

circle = sketch.addGeometry(
    Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 1), False)

# Set radius to 8 mm
sketch.addConstraint(Sketcher.Constraint("Radius", circle, 8.0))
doc.recompute()
```

### Method signature

```python
sketch.addConstraint(Sketcher.Constraint("Radius", geoId, value))
# 'value' is always the radius regardless of whether you display as Ø.
# To display as diameter in the UI, the constraint display mode is a UI-only setting.
```

---

## See also

- [Dimension](dimension.md) — unified tool; use `M` to toggle between R and Ø
- [Equal](constraint-equal.md) — force two circles to have equal radius
- [Distance](constraint-distance.md) — set linear distances
- [Create Circle / Ellipse](create-circle.md) — draw the circles to constrain
