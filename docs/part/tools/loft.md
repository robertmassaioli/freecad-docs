# Loft

> **In one sentence:** Blend smoothly through a series of 2-D cross-section
> profiles to create a solid or shell whose shape transitions from one
> cross-section to the next.

---

## Overview

**Workbench:** Part · **Menu:** Part → Loft  
**Toolbar:** Part Modeling · **Shortcut:** none

Loft creates a solid or surface by connecting a sequence of closed (or open)
profile wires. Unlike [Ruled Surface](ruled-surface.md) (which connects only
two rails with straight generators), Loft can connect any number of profiles
and uses smooth (B-spline) blending.

---

## Intuition

An aircraft fuselage transitions from a circular nose cross-section through
various oval sections to a different tail cross-section. A Loft operation
models exactly this: you provide the cross-sections at key stations and
FreeCAD blends smoothly between them.

Unlike [Sweep](sweep.md) — which takes a constant cross-section and moves it
along a path — Loft takes *changing* cross-sections and threads them together.
You can combine both by providing cross-sections at different Z heights and
letting Loft figure out the spine.

---

## When to use it

- Aerodynamic or ergonomic shapes that transition between cross-sections.
- Bottle or vase profiles that change shape from base to neck to rim.
- Blending a round tube into a rectangular duct.

## When NOT to use it

- **Constant cross-section along a path** — use [Sweep](sweep.md).
- **Two rails only, straight lines** — use [Ruled Surface](ruled-surface.md)
  for a simpler, potentially more stable result.

---

## Step-by-step walkthrough

1. **Create the cross-section profiles.** Each must be a closed wire or sketch
   lying in a plane roughly perpendicular to the loft direction.
2. **Open the Loft dialog:** Part → Loft.
3. In the dialog, **add profiles** in order (first to last) using the **Add**
   button.
4. Set options:
   - **Ruled** — straight (ruled) transitions between profiles (= Ruled Surface
     generalised to N sections).
   - **Closed** — connect the last profile back to the first.
   - **Create Solid** — closed ends produce a solid; open ends produce a shell.
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Profiles | List of wires | — | Cross-section profiles in order |
| Ruled | Bool | No | Straight (ruled) vs smooth (B-spline) blending |
| Closed | Bool | No | Connect last profile back to first |
| Create Solid | Bool | Yes | Solid vs open shell |

---

## Deep dive: Ruled vs smooth blending

#### Ruled mode

**Intuition:** Straight-line generators connect corresponding points on
successive profiles. Computationally cheaper and guaranteed to stay between
the profiles. Produces faceted (polyhedral) surfaces when profiles differ
significantly.

Use ruled mode when:
- The shape must be developable (unfoldable) for sheet metal.
- You want maximum control with no unexpected curvature.
- Speed matters and the approximation is acceptable.

#### Smooth mode (default)

**Intuition:** OCCT fits B-spline surfaces through the profiles, honouring
tangency at each end profile if the profile has an adjacent face to be tangent
to. Produces continuously curved surfaces.

Use smooth mode (default) when:
- The lofted shape must look organic, aerodynamic, or fully smooth.
- You plan to use the result in CFD, rendering, or Class-A surface work.

---

## Common mistakes and pitfalls

!!! warning "Profile order determines shape"
    Profiles are blended in the order you add them. Adding them out of order
    produces a self-intersecting loft. Always add profiles from one end to
    the other.

!!! warning "Profiles must have compatible topology"
    Blending a circle (1 edge) with a square (4 edges) requires OCCT to map
    one circular arc to each of the four square sides. The mapping may produce
    unexpected transitions. Use a profile with the same number of edges as the
    target, or use a smooth round-rectangle ("squircle") intermediate profile.

!!! warning "Closed loft requires at least 3 profiles"
    Closing a loft with only 2 profiles produces a degenerate result. Add a
    third intermediate profile.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Three profiles at different Z heights
circle_bottom = Part.Wire([Part.makeCircle(10, App.Vector(0, 0, 0), App.Vector(0, 0, 1))])
circle_mid    = Part.Wire([Part.makeCircle(15, App.Vector(0, 0, 15), App.Vector(0, 0, 1))])
circle_top    = Part.Wire([Part.makeCircle(5,  App.Vector(0, 0, 30), App.Vector(0, 0, 1))])

loft = Part.makeLoft([circle_bottom, circle_mid, circle_top],
                     solid=True, ruled=False, closed=False)

feat = doc.addObject("Part::Feature", "LoftedSolid")
feat.Shape = loft
doc.recompute()
print(f"Volume: {feat.Shape.Volume:.2f} mm³")
```

### Parametric Loft object

```python
import FreeCAD as App

doc = App.newDocument()
# Create sketch profiles in the document...
# sketch1 = ... sketch2 = ... sketch3 = ...

loft_obj = doc.addObject("Part::Loft", "Loft")
loft_obj.Sections = [sketch1, sketch2, sketch3]
loft_obj.Solid    = True
loft_obj.Ruled    = False
loft_obj.Closed   = False
doc.recompute()
```

---

## See also

- [Sweep](sweep.md) — constant cross-section along a path
- [Ruled Surface](ruled-surface.md) — straight-line loft between two rails
- [Extrude](extrude.md) — single-profile linear extrusion
