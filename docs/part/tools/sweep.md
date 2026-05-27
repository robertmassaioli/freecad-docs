# Sweep

> **In one sentence:** Move a cross-section profile along a spine path to
> create a solid or surface — the profile's shape stays constant while its
> position and orientation follow the path.

---

## Overview

**Workbench:** Part · **Menu:** Part → Sweep  
**Toolbar:** Part Modeling · **Shortcut:** none

Sweep creates a solid or shell by moving one or more closed profiles along a
3-D spine path. The profiles are placed perpendicular to the spine at each
point, creating the swept volume.

---

## Intuition

Think of squeezing toothpaste: the nozzle (profile) is constant; the tube of
toothpaste (spine) determines where the material goes. The bead of toothpaste
is the Sweep result.

Common swept shapes:
- **Pipes and tubing** — circle profile along a 3-D spline spine.
- **Springs** — circle profile along a [Helix](primitives.md) spine.
- **Channels and extrusions** — custom cross-section along a straight or curved
  path.
- **Wiring looms** — oval profile following cable routing paths.

---

## When to use it

- Any profile you want to route through 3-D space along a defined path.
- Springs: circle profile along a Part Helix.
- Pipe bends: circle profile along a 3-D arc or spline.

## When NOT to use it

- **Changing cross-section** — use [Loft](loft.md) instead.
- **Linear extrusion** — use [Extrude](extrude.md) (simpler and more stable).

---

## Step-by-step walkthrough

1. **Create the spine path** — a wire, edge, or sketch that defines the path.
   It can be 3-D (a spline, helix, arc sequence).
2. **Create the profile** — a closed wire or sketch perpendicular to the start
   of the spine.
3. **Open Part → Sweep.**
4. In the dialog:
   - **Add the profile** (click the profile, then **Add**).
   - **Set the spine** (click the spine object, then the **Spine** button).
   - Configure **Frenet** mode and **Transition** options.
5. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Sections | List of wires | — | Cross-section profiles to sweep |
| Spine Path | Wire | — | The path to sweep along |
| Create Solid | Bool | Yes | Solid vs open shell |
| Frenet | Bool | No | Use Frenet frame to orient the profile along the path |
| Transition | Enum | Right Corner | How profile handles sharp corners in the spine: Right Corner / Round Corner / Transformed |

---

## Deep dive: Frenet mode and Transition options

#### Frenet mode

**Intuition:** Without Frenet, the profile orientation is determined by the
minimum rotation frame, which can produce twisting for certain paths. With
Frenet on, the profile is oriented by the path's tangent and normal vectors
(the Frenet–Serret frame), which avoids twist for planar paths but may
introduce twist for highly 3-D paths.

Use Frenet for:
- Paths that lie in a single plane (2-D arcs, circles).
- Situations where the "up" direction of the profile should follow the path
  curvature naturally.

#### Transition: Right Corner

**Intuition:** At sharp corners in the spine, the profile is simply
moved/rotated to the new direction — a miter joint. Produces sharp corners in
the swept solid.

#### Transition: Round Corner

**Intuition:** At sharp corners, FreeCAD inserts an arc to smooth the
transition, producing a rounded corner in the swept solid.

#### Transition: Transformed

**Intuition:** The profile is scaled or sheared at corners to fit the
transition. Less commonly needed.

---

## Common mistakes and pitfalls

!!! warning "Profile must be at the start of the spine"
    Place the profile's plane at the starting point of the spine. If the
    profile is far from the spine start, the sweep result will be offset or
    misaligned.

!!! warning "Spine must be a connected wire, not separate edges"
    If the spine consists of several disconnected edges, assemble them into a
    wire using [Shape Builder](shape-builder.md) first.

!!! warning "Self-intersecting sweep when spine curves too sharply"
    If the spine has a curve with a radius smaller than the profile's extent,
    the swept solid self-intersects. Increase the spine radius or reduce the
    profile size.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Profile: small circle at the origin (XY plane)
circle_edge = Part.makeCircle(3.0, App.Vector(0, 0, 0), App.Vector(0, 0, 1))
profile = Part.Wire([circle_edge])

# Spine: a helix
helix = doc.addObject("Part::Helix", "Helix")
helix.Pitch  = 8.0    # mm
helix.Height = 40.0   # mm
helix.Radius = 12.0   # mm
doc.recompute()

# Sweep via Part object
sweep = doc.addObject("Part::Sweep", "Spring")
sweep.Sections  = [doc.addObject("Part::Feature", "Profile")]
doc.getObject("Profile").Shape = profile

sweep.Spine     = (helix, ["Edge1"])   # spine from helix
sweep.Solid     = True
sweep.Frenet    = True
doc.recompute()
```

> **Practical note:** For a spring sweep, create the helix with the
> Primitives dialog, create a small circle Sketcher sketch on the XY plane,
> then run Sweep interactively — it is easier than the scripted approach.

---

## See also

- [Loft](loft.md) — changing cross-section (multiple profiles)
- [Extrude](extrude.md) — straight-line sweep (simpler, more stable)
- [Primitives → Helix](primitives.md) — common spine for spring sweeps
