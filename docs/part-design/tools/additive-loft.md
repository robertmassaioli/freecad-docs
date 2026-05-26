# Additive Loft

> **In one sentence:** Blend a solid smoothly through two or more cross-section
> sketches to create a shape that transitions from one profile to the next.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Features → Additive Loft  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Additive Loft takes a *profile* sketch and one or more *section* sketches and
constructs a solid that smoothly interpolates between them in order. The result
is fused into the active Body, adding material. Unlike Additive Pipe, which
requires a spine to define the path, Additive Loft connects the sections
directly, inferring the transition shape between them. Use it whenever you need
a solid that changes cross-sectional shape from one plane to another.

---

## Intuition

Imagine a soft lump of clay connecting two wire frames: a circle at the bottom
and a square at the top. The clay bulges out to fill both shapes at each end
and creates a smooth transition in between. That transition is the loft.

The key insight is that Additive Loft does not need a path sketch — it finds
its own route between the sections by interpolating through space. The order
in which you add the sections determines the sequence of the transition; the
loft always travels from the profile to the first section, then to the second,
and so on.

A practical consequence: the spatial positioning of each section sketch matters
enormously. Each section must be placed on a different plane (datum plane or
face) so the loft has somewhere to go. Sections on the same plane, or very
close together, produce extremely thin geometry that often fails.

---

## When to use it

- You need a solid that transitions between two different cross-sections — for
  example, a nozzle that is circular at the inlet and rectangular at the outlet.
- You are modelling an ergonomic shape like a handle or grip that is wider at
  one end than the other.
- You want to blend through several intermediate cross-sections with full
  control over the shape at each station.
- The profile and sections are all closed wires (the loft produces a closed
  solid rather than an open shell).
- You need to close a shape back to its starting profile — use the *Closed*
  option to create a torus-like body that loops from the last section back to
  the first.

---

## When NOT to use it

- **The cross-section is constant along the path** — use [Pad](pad.md) for
  straight paths or [Additive Pipe](additive-pipe.md) for curved paths; they
  are simpler and more robust.
- **The shape follows a specific curved spine** — use
  [Additive Pipe](additive-pipe.md), which gives you explicit control over the
  path the profile travels.
- **The shape revolves around an axis** — use [Revolution](revolution.md),
  which is simpler and more predictable for axisymmetric solids.
- **You want to remove material** — use
  [Subtractive Loft](subtractive-loft.md), which has identical parameters but
  cuts instead of adds.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least two fully-constrained sketches
placed on different planes. The *profile* is the starting cross-section; the
*sections* are the subsequent cross-sections in order.

1. **Create the profile sketch.** Draw and fully constrain the starting
   cross-section. This sketch will be the "first" shape in the loft sequence.
   Close the Sketcher task panel.

2. **Create section sketches.** For each additional cross-section, add a datum
   plane at the desired position along the loft, then create and constrain a
   sketch on that plane. Each section must have the same number of closed wires
   as the profile (or exactly one point as the final section to close to a tip).
   Close each Sketcher task panel after completing it.

3. **Activate Part Design → Additive Features → Additive Loft.** The task panel
   opens with two checkboxes (*Ruled surface*, *Closed*), a *Profile* group,
   and *Add Section* / *Remove Section* buttons.

4. **Set the Profile.** In the *Profile* group, click **Object** and then click
   the starting profile sketch in the model tree or viewport. The sketch name
   appears in the text field.

5. **Add sections.** Click **Add Section**, then click the first section sketch.
   Click **Add Section** again for each additional section, selecting them in
   the order you want the loft to travel through them. The section names appear
   in the list widget.

6. **Reorder sections if needed.** The list widget supports drag-and-drop
   reordering. The loft always travels from the profile through the list in
   top-to-bottom order.

7. **Enable Ruled surface or Closed** if required (see [Parameters](#parameters)).

8. **Click OK.** FreeCAD blends through all the sections and fuses the result
   into the Body.

!!! tip
    You can remove a section from the loft by selecting it in the list and
    clicking **Remove Section**. The profile itself cannot be removed via the
    sections list — edit the `Profile` property directly if you need to change
    the starting sketch.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Ruled surface | Checkbox | Off | When on, each span between adjacent sections is a ruled surface (straight lines connecting corresponding points on adjacent profiles). Produces a faceted, prismatic appearance instead of a smooth blend. |
| Closed | Checkbox | Off | When on, the loft adds a final span connecting the last section back to the profile, forming a closed loop. Requires at least three sections (profile plus two more); automatically disabled with fewer sections. |
| Profile | Object link | — | The first cross-section sketch. Sets the starting shape of the loft. |
| Add Section / Remove Section | Buttons | — | Toggle selection mode to add or remove section sketches from the ordered list. |
| Sections list | Ordered list | — | The section sketches in loft order. Drag and drop to reorder. All sections must have the same number of closed wires as the profile; only the last section may be a single point (to taper to a tip). |

---

## Advanced usage

### Tapering to a point

The last section in the list may be a single vertex (a point sketch) rather
than a closed wire. This causes the loft to taper to a sharp tip at that end,
producing cone-like or pyramid-like shapes. The profile and all intermediate
sections must still be closed wires.

### Closed lofts

When *Closed* is enabled the loft adds a span from the last section back to the
profile, creating a continuous closed solid. This is useful for torus-like shapes
or solid blobs that must loop around. At least three profiles (profile + two
sections) are required; with only two sections FreeCAD silently ignores the
flag.

### Matching wire count

Each section must have **exactly the same number of closed wires** as the
profile. A single-loop circle and a single-loop square can loft together
(both have one wire). A circle and a figure-eight (two loops) cannot — FreeCAD
will report a shape error. If you need to loft between profiles with different
topology, break the operation into separate Additive Loft features and join
them.

---

## Common mistakes and pitfalls

!!! warning "At least one section is needed"
    **Cause:** Clicking OK without adding any sections. The profile alone is
    not enough to define a loft — there is nothing to loft *to*.  
    **Fix:** Add at least one section sketch using the **Add Section** button.

!!! warning "Sections have different wire counts"
    **Cause:** The profile has (for example) two closed wires (a ring with an
    inner and outer boundary) but a section has only one. The loft engine
    cannot match them.  
    **Fix:** Ensure every section sketch has the same number of closed loops as
    the profile, or adjust the profile.

!!! warning "Failed to create shell / Resulting shape is not a solid"
    **Cause:** Two sections are on the same plane or very close together, the
    sections cross each other in 3-D space, or a section is not closed.  
    **Fix:** Confirm that each section is on a distinctly different plane. Check
    that all sections are fully closed. Check that the section outlines do not
    visually overlap when viewed from the side.

!!! warning "Section sketches placed in the wrong order"
    **Cause:** The loft travels through sections in list order. If sections are
    listed out of spatial order the solid twists or self-intersects.  
    **Fix:** Drag the entries in the sections list to match the physical order
    from the profile outward.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create the body
body = doc.addObject("PartDesign::Body", "Body")

# Profile sketch: circle of radius 10 mm on the XY plane (Z=0)
profile = doc.addObject("Sketcher::SketchObject", "Profile")
body.addObject(profile)
profile.addGeometry(Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 10), False)
profile.addConstraint(Sketcher.Constraint("Coincident", 0, 3, -1, 1))
doc.recompute()

# Datum plane at Z=40
plane = doc.addObject("PartDesign::Plane", "DatumPlane")
body.addObject(plane)
plane.MapMode = "FlatFace"
plane.AttachmentSupport = (doc.XY_Plane, [""])
plane.MapPathParameter = 0.0
plane.AttachmentOffset.Base.z = 40.0
doc.recompute()

# Section sketch: square 15×15 mm on the datum plane at Z=40
section = doc.addObject("Sketcher::SketchObject", "Section")
body.addObject(section)
section.MapMode = "FlatFace"
section.AttachmentSupport = (plane, [""])
doc.recompute()
# (add geometry + constraints for a 15x15 square centred at origin)  # TODO: verify

# Create the Additive Loft
loft = doc.addObject("PartDesign::AdditiveLoft", "AdditiveLoft")
body.addObject(loft)
loft.Profile = profile
loft.Sections = [(section, [])]
loft.Ruled = False
loft.Closed = False
doc.recompute()

print(f"Volume: {loft.Shape.Volume:.4f} mm³")
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The first (starting) cross-section sketch. Inherited from `ProfileBased`. |
| `Sections` | `App::PropertyLinkSubList` | `[]` | The ordered list of section sketches (everything after the profile). |
| `Ruled` | `App::PropertyBool` | `False` | Generate ruled (straight-line) spans between sections instead of smooth curves. |
| `Closed` | `App::PropertyBool` | `False` | Close the loft by adding a final span from the last section back to the profile. Silently ignored if fewer than three profiles total are provided. |

---

## See also

- [Subtractive Loft](subtractive-loft.md) — identical parameters, removes material instead
- [Additive Pipe](additive-pipe.md) — sweeps a profile along an explicit spine curve
- [Additive Helix](additive-helix.md) — sweeps along a helical path
- [Revolution](revolution.md) — simpler rotation for axisymmetric shapes
- [Pad](pad.md) — straight extrusion when the cross-section does not change
