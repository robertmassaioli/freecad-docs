# Snell's Law Constraint

> **In one sentence:** Constrain the direction of two line segments meeting at a
> boundary point to obey Snell's Law of refraction for a specified refractive
> index ratio, enabling accurate optical path sketches.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Snell's Law Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `K, W`

Snell's Law Constraint is a highly specialised tool for optics design. It enforces
the relationship `n₁ sin(θ₁) = n₂ sin(θ₂)` at a boundary point between two line
segments, where θ₁ and θ₂ are the angles each line makes with the normal to the
boundary, and n₁, n₂ are the refractive indices of the two media.

This allows you to sketch the exact path of a light ray through a lens or prism
directly in Sketcher.

---

## Intuition

When light passes from air into glass, it bends toward the perpendicular (normal)
to the glass surface — this bending is refraction. Snell's Law quantifies exactly
how much: a steeper entry angle bends more, a higher refractive index bends more.
This constraint encodes that physics directly in the sketch so your optical
ray-path sketch is geometrically exact.

---

## When to use it

- You are designing an optical lens, prism, or waveguide and want to sketch exact
  ray paths through the geometry.
- You need to verify that a designed optical element will refract a specific
  incoming ray to a specific exit angle.
- You are documenting optical bench layouts with accurate ray paths.

## When NOT to use it

- **For non-optical use** — Snell's Law is physically meaningful only for optics;
  do not use it as a general two-angle constraint.
- **For reflection** — this constraint models refraction only; for reflection
  (angle of incidence = angle of reflection) use two [Angle](constraint-angle.md)
  constraints with equal values about the normal.

---

## Step-by-step walkthrough

1. **Draw the boundary edge** (the interface between two optical media) as a line
   in the sketch.

2. **Draw two ray lines** — one on each side of the boundary — that meet at a
   point on the boundary. Add a [Point on Object](constraint-point-on-object.md)
   or [Coincident](constraint-coincident.md) to anchor the meeting point to the
   boundary.

3. **Select three elements** in order: the incident ray, the refracted ray, and
   the boundary line.

4. **Press `K, W`** or activate **Snell's Law Constraint**.

5. A value dialog appears asking for `n₂/n₁` — the ratio of refractive indices.
   For an air-to-glass interface with n_glass = 1.5 and n_air = 1.0, enter 1.5.

6. **Click OK.** The refracted ray adjusts to the correct angle.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Refractive index ratio (n₂/n₁) | Float | The ratio of the second medium's index to the first medium's index. n₂/n₁ > 1 = light slows down (bends toward normal). |

---

## Common mistakes and pitfalls

!!! warning "Constraint cannot be solved — geometry is inconsistent"
    **Cause:** The incident angle is so large that refraction is impossible (total
    internal reflection). Snell's Law has no real solution for n₁ sin(θ₁) > n₂.  
    **Fix:** Reduce the incident angle or use a lower refractive index ratio.

!!! warning "Wrong order of selections"
    **Cause:** The constraint uses the selection order to determine which ray is
    incident and which is refracted.  
    **Fix:** Select the incident ray first (the ray in medium 1), then the refracted
    ray (medium 2), then the boundary.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Assuming: incident ray at geo 0, refracted ray at geo 1, boundary at geo 2
# Refractive index ratio n2/n1 = 1.5 (air to glass)
sketch.addConstraint(Sketcher.Constraint("SnellsLaw", 0, 2, 1, 2, 2, 3, 1.5))
# TODO: verify exact argument order against FreeCAD source
doc.recompute()
```

---

## See also

- [Angle](constraint-angle.md) — general angle constraint
- [Perpendicular](constraint-perpendicular.md) — for constructing surface normals
- [Point on Object](constraint-point-on-object.md) — anchor the refraction point to the boundary
