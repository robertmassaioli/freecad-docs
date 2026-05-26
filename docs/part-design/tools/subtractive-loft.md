# Subtractive Loft

> **In one sentence:** Blend a profile sketch through one or more section sketches to cut a smoothly
> transitioning void out of an existing Body.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Features → Subtractive Loft  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Subtractive Loft takes a starting profile and one or more additional section sketches and removes
the blended solid formed between them from the active Body. The profile can change shape from
section to section, allowing the cut to taper, flare, or morph as it travels through the part.
It is the subtractive counterpart to [Additive Loft](additive-loft.md), which adds material
instead of removing it.

---

## Intuition

Imagine pushing a clay-cutter of one shape into a block of clay, but as the cutter descends its
outline gradually transforms into a different shape at the bottom. The cavity left behind takes a
form that smoothly interpolates between both outlines — wider at the top, narrower or differently
shaped at the bottom.

Subtractive Loft does exactly this, driven purely by sketches. You define the entry shape (the
*profile*), one or more intermediate shapes (*sections*), and FreeCAD constructs a solid by
skinning a smooth surface through all of them. That solid is then subtracted from the Body.

The key insight is that the sections do not need to be parallel. Each section can live on a
different datum plane at any angle or position, allowing the loft to curve through 3-D space. This
makes it the right choice for organic cutouts and blended pockets that no other tool can produce.

---

## When to use it

- You need a pocket that transitions from one cross-section shape to another (e.g. a round inlet
  that widens into a rectangular chamber).
- You are cutting a blended recess that must smoothly match two different mating geometry outlines
  at different depths.
- You need an aerodynamic or ergonomic cutout whose profile changes along a non-straight axis.
- You want to remove a complex tapered cavity that would require multiple individual Pocket
  operations if modelled any other way.
- The cut must terminate at a point (closing the loft to a vertex), such as the tip of an inset
  blade socket.

---

## When NOT to use it

- **The cut has a constant cross-section along a straight path** — use [Pocket](pocket.md) instead;
  it is simpler and less prone to topology failure.
- **The cut revolves around a central axis** — use [Groove](groove.md), which is purpose-built for
  axisymmetric cuts.
- **The profile travels along a specific spine curve** — use
  [Subtractive Pipe](subtractive-pipe.md), which lets you define the path explicitly.
- **The cut follows a helical path** — use
  [Subtractive Helix](subtractive-helix.md).
- **You want to add material** — use [Additive Loft](additive-loft.md).

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one existing solid feature to cut into. You need at
least two sketches: the *profile* (starting cross-section) and at least one additional *section*.
All sketches must belong to the Body before you start.

1. **Create the profile sketch.** Draw and fully constrain the starting cross-section on any plane
   or datum plane. The profile must be a single closed wire (or nested closed wires for a hollow
   profile). Close the Sketcher task panel.

2. **Create at least one section sketch.** Draw the ending (or intermediate) cross-section on a
   *different* plane. The section can be any shape, but must have a wire count compatible with the
   profile (same number of closed loops). To close the loft to a point, use a single vertex instead
   of a closed wire. Close the Sketcher task panel.

3. **Select the profile sketch** in the model tree, then activate **Part Design → Subtractive
   Features → Subtractive Loft**. The task panel opens with the profile already populated.

4. **Add sections.** In the *Sections* list, click **Add section**, then click each additional
   sketch in the model tree or viewport in the order you want the loft to pass through them.
   The order in the list determines the direction of blending.

5. **Set Ruled surface** (optional). Toggle **Ruled surface** on if you want straight ruled
   (non-curved) faces between sections rather than smooth NURBS blending.

6. **Set Closed** (optional). Toggle **Closed** on to blend the last section back to the first,
   creating a closed loop. Requires at least three sections.

7. **Click OK.** FreeCAD constructs the loft solid and subtracts it from the Body. Check the model
   tree — the new `SubtractiveLoft` feature appears and the Body's shape is updated.

!!! tip
    The order sections are added to the list matters. If the loft twists unexpectedly, try
    reordering sections by selecting a row and using the up/down arrows, or by removing and
    re-adding them in the correct order.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Profile | Object link | — | The starting sketch or sub-element. Must be a closed wire (or vertex for a point termination). |
| Sections | Object link list | — | One or more additional sketches through which the loft is blended, in path order. Each section must have the same wire count as the profile, except the final section which may be a single vertex. |
| Add section / Remove section | Buttons | — | Toggle selection mode to add or remove sections from the list. |
| Ruled surface | Checkbox | Off | When on, faces between adjacent sections are ruled (straight-line) surfaces. When off, smooth curved (NURBS) surfaces are used. |
| Closed | Checkbox | Off | When on, the loft wraps from the last section back to the first, forming a closed loop. Requires at least three sections (two sections plus the profile). |

---

## Advanced usage

### Closing the loft to a point

Any section in the list can be a single vertex (a point sketch). If the final section is a vertex,
the loft tapers to a sharp point. This is useful for modelling pointed sockets, blade-tip slots, or
taper-ended cavities:

1. Draw a sketch with a single constrained point (use the **Point** geometry tool in Sketcher).
2. Add this single-point sketch as the last section.

The loft will taper from the profile shape to zero at that point.

### Lofting between non-parallel planes

Each section can sit on a completely different datum plane, allowing the cut to travel in any
direction through the Body, not just along a straight axis. Place datum planes at the positions and
angles you need, draw a section sketch on each, and add them in order to the section list.

### Ruled surface for sharp-edged cuts

The default smooth (NURBS) blending can produce faces that are hard to predict when sections have
very different shapes. Switching to **Ruled surface** forces each pair of adjacent section wires
to be connected by straight lines. The result is less organic but more geometrically predictable
and less likely to produce self-intersecting faces.

---

## Common mistakes and pitfalls

!!! warning "No sections added / 'At least one section is needed'"
    **Cause:** The Sections list is empty. The loft needs at least one section beyond the profile
    to form a solid.  
    **Fix:** Click **Add section** and select at least one additional sketch.

!!! warning "Section wire count mismatch"
    **Cause:** The profile has a different number of closed wires than one of the sections (e.g.
    a hollow profile with two wires versus a solid section with one).  
    **Fix:** Ensure all sections have the same number of wires as the profile, or that only the
    final section is a single vertex.

!!! warning "Loft: Failed to create shell / Result is not a solid"
    **Cause:** The loft geometry is self-intersecting, typically because sections are very close
    together, have opposing orientations, or the wires cross over each other.  
    **Fix:** Check section orientations, increase spacing between sections, or switch to **Ruled
    surface** to simplify the face construction.

!!! warning "Profile sketch is not closed"
    **Cause:** The profile sketch has an open wire, which cannot form a solid face.  
    **Fix:** Open the profile sketch, close the gap, and ensure the Sketcher reports zero open
    edges.

!!! warning "Unexpected twist between sections"
    **Cause:** OCCT matches vertices between adjacent section wires by proximity. If the wires
    are oriented differently (one clockwise, one counter-clockwise, or start points far apart),
    the surface can twist.  
    **Fix:** Edit the section sketches to ensure all wires run in the same direction and that
    corresponding vertices are in roughly the same angular position relative to the wire centre.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create the body with a base pad to cut into
body = doc.addObject("PartDesign::Body", "Body")

base_sk = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sk)
base_sk.addGeometry(Part.makePolygon([
    App.Vector(-20, -20, 0), App.Vector(20, -20, 0),
    App.Vector(20, 20, 0), App.Vector(-20, 20, 0),
    App.Vector(-20, -20, 0)
]))
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = base_sk
pad.Length = 40.0
doc.recompute()

# Profile sketch: 10 mm circle on XY plane
profile = doc.addObject("Sketcher::SketchObject", "Profile")
body.addObject(profile)
profile.addGeometry(Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 10), False)
profile.addConstraint(Sketcher.Constraint("Coincident", 0, 3, -1, 1))
doc.recompute()

# Section sketch: 5 mm circle, 30 mm up on XY plane at z=30
section_plane = doc.addObject("PartDesign::Plane", "DatumPlane")
body.addObject(section_plane)
section_plane.MapMode = "FlatFace"
section_plane.AttachmentOffset = App.Placement(
    App.Vector(0, 0, 30), App.Rotation()
)
doc.recompute()

section = doc.addObject("Sketcher::SketchObject", "Section")
body.addObject(section)
section.AttachmentSupport = (section_plane, [""])
section.MapMode = "FlatFace"
doc.recompute()
section.addGeometry(Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 5), False)
section.addConstraint(Sketcher.Constraint("Coincident", 0, 3, -1, 1))
doc.recompute()

# Create the Subtractive Loft  # TODO: verify
loft = doc.addObject("PartDesign::SubtractiveLoft", "SubtractiveLoft")
body.addObject(loft)
loft.Profile = profile
loft.Sections = [section]
doc.recompute()
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The starting profile sketch or sub-element. Inherited from `ProfileBased`. |
| `Sections` | `App::PropertyLinkSubList` | `[]` | Additional section sketches in path order. |
| `Ruled` | `App::PropertyBool` | `False` | When `True`, faces between sections are ruled (straight-line) rather than smooth NURBS. |
| `Closed` | `App::PropertyBool` | `False` | When `True`, the last section blends back to the first, forming a closed loop. |

---

## See also

- [Additive Loft](additive-loft.md) — identical parameters, adds material instead of removing it
- [Subtractive Pipe](subtractive-pipe.md) — removes material swept along an explicit spine curve
- [Subtractive Helix](subtractive-helix.md) — removes material swept along a helix path
- [Pocket](pocket.md) — simpler straight-line cut for constant cross-sections
- [Groove](groove.md) — axisymmetric cut around a central axis
