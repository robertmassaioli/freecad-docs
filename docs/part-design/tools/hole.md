# Hole

> **In one sentence:** Cut a standards-aware cylindrical hole into a Body, with
> built-in support for threads, countersinks, counterbores, and drill-point
> geometry in one unified tool.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Features → Hole  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Hole is Part Design's most feature-rich subtractive operation. Given a sketch
containing one or more circles (or points), it drills matching holes into the
Body — optionally with threads, a countersink or counterbore recess for a screw
head, a drill-point cone at the bottom, or a taper along the wall. All thread
geometry follows published fastener standards (ISO metric, UNC, UNF, UNEF, BSW,
BSP, NPT, and others), so you never need to look up tap-drill diameters or
counterbore dimensions manually.

---

## Intuition

Think of the Hole tool as a CNC drilling and tapping cycle packed into a single
dialog. A machinist drilling a hole for an M6 socket-head cap screw must choose:
the tap-drill diameter (5 mm), the counterbore diameter for the screw head, the
counterbore depth, and the thread depth. The Hole tool automates all of those
choices the moment you pick `ISOMetricProfile` and `M6x1.0` — the rest of the
fields fill in with ISO-standard values that you can override.

The sketch driving the hole carries the *position* of each hole centre. The
Hole task panel carries every other dimension. One hole feature can produce
dozens of identical holes simultaneously: FreeCAD finds every circle (or point)
in the sketch and cuts a hole at each one.

A subtlety that confuses new users: the *hole depth* and the *thread depth* are
controlled separately. You can have a 20 mm deep hole with only 15 mm of thread,
which is the normal engineering practice (partial-thread blind hole). The
`Thread Depth Type` dropdown controls this relationship — setting it to `Hole
Depth` makes the thread run the full depth; `Dimension` lets you set an
independent thread depth; `Tapped (DIN76)` adds a DIN 76 thread run-out
allowance automatically.

---

## When to use it

- You need a bolt or screw to thread into the part, and you want the correct
  tap-drill diameter and thread class selected according to an industry standard.
- You are preparing a clearance hole for a fastener to pass through, and you
  want a correctly sized counterbore or countersink for the screw head.
- You need several identical holes placed at multiple positions defined by a
  sketch (one Hole feature can create all of them at once).
- You want the 3-D model to show an accurate helical thread form for technical
  presentation or FEA (enable `Modeled thread`).
- You need a non-standard simple cylindrical blind or through hole of a given
  diameter — the tool works without any thread standard selected.

---

## When NOT to use it

- **The opening is not a circular hole** — use [Pocket](pocket.md) for
  rectangular, slotted, or other non-circular cutouts.
- **You need a rotationally tapered pipe thread (NPT or BSP taper)** — the Hole
  tool can size the hole for those standards, but does not generate a tapered
  bore geometry; model the taper manually with [Revolution](revolution.md) or a
  tapered Pocket.
- **You need a partial-sphere or non-cylindrical recessed seat** — use a
  [Pocket](pocket.md) with the appropriate sketch profile instead.
- **The hole axis is not perpendicular to a planar face** — Hole always drills
  perpendicular to the attachment plane of its sketch. For angled holes, tilt the
  datum plane and attach the sketch to that.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one solid feature (e.g. a Pad or
an Additive Box) and a face to drill into.

1. **Create the position sketch.** Select a planar face of the Body, then open
   **Sketch → New Sketch**. Draw one circle for each hole position. The circle's
   radius is ignored by the Hole tool (the task panel controls diameter), but
   placing a circle rather than a point allows the Hole tool to read the sketch
   by default. Fully constrain all centres. Close the Sketcher.

    !!! tip
        If you prefer to mark hole centres with sketch points instead of circles,
        set **Base profile types** to `Points, circles and arcs` or `Points` in
        the task panel after activating the Hole tool.

2. **Activate the Hole tool.** Open **Part Design → Subtractive Features → Hole**,
   or click the Hole icon in the toolbar. The *Hole Parameters* task panel opens.

3. **Choose a thread standard.** In the **Standard** dropdown, select the
   fastener family: `None` for a plain dimensioned hole, or one of the thread
   standards (e.g. `ISO metric regular`). When a standard is selected the
   **Size** and **Head type** rows become active.

4. **Select the size.** Pick the nominal designation in the **Size** dropdown
   (e.g. `M6x1.0`). FreeCAD sets the tap-drill diameter and thread pitch
   automatically.

5. **Set the head type.** In **Head type** choose `None`, `Counterbore`,
   `Countersink`, or `Counterdrill`. For a standard screw type (e.g. ISO 4762
   socket-head cap screw) FreeCAD fills the counterbore or countersink dimensions
   from the norm. Tick **Custom head values** to override those dimensions.

6. **Set the depth type and depth.** In **Depth type** choose `Dimension` for a
   blind hole or `Through all` for a through-hole. If `Dimension` is selected,
   enter the hole depth in the **Depth** field.

7. **Configure the drill point.** Tick **Drill angle** to add a conical drill-
   point at the bottom (typical for twist-drilled blind holes). Enter the
   included angle in **Drill angle** (118° is the standard for high-speed-steel
   twist drills). Tick **Include in depth** if the drill-point cone should be
   counted as part of the nominal hole depth.

8. **Set the Threading section** (visible when a thread standard is selected).

    a. In **Hole type**, choose `Clearance / Passthrough` for a non-threaded
       clearance hole, `Tap drill (to be threaded)` for a tapped hole without
       modelled thread geometry, or `Modeled thread` to generate a real 3-D helix.

    b. If threaded, choose **Thread Depth Type**: `Hole depth` (thread fills the
       hole), `Dimension` (enter an explicit depth), or `Tapped (DIN76)` (DIN 76
       runout allowance is added automatically).

    c. Select **Class** (tolerance class, e.g. `6H` for ISO metric).

    d. Select **Direction**: `Right hand` or `Left hand`.

    e. For modeled threads, optionally tick **Custom Clearance** and enter a
       clearance offset to adjust thread fit for manufacturing tolerance.

9. **Optionally enable Tapered.** Tick **Tapered** and enter an angle in
   **Tapered angle** to produce a conical hole wall. 90° means a straight
   (non-tapered) hole, below 90° the hole narrows toward the bottom.

10. **Click OK.** FreeCAD cuts a hole at every circle (or point) in the sketch
    and fuses the result into the Body. The model tree shows a single `Hole`
    feature regardless of how many holes were created.

!!! tip
    For a standard M6 tapped blind hole to accept a socket-head cap screw (ISO
    4762), select `ISO metric regular`, size `M6x1.0`, head type `Counterbore`,
    hole type `Tap drill (to be threaded)`, depth type `Dimension`, depth 15 mm,
    drill angle ticked at 118°. All counterbore dimensions are filled
    automatically from ISO 4762.

---

## Parameters

The task panel is organised into logical groups, documented below in the order
they appear in the UI.

### Profile and geometry

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Base profile types | Dropdown | `Circles and arcs` | Which sketch elements define hole centres. `Circles and arcs`: circles and arcs only. `Points, circles and arcs`: centres of all three. `Points`: sketch points only. Corresponds to `BaseProfileType` bitmask property. |
| Standard | Dropdown | `None` | Thread standard. `None`: plain dimensioned hole. Other values: `ISO metric regular` (ISOMetricProfile), `ISO metric fine` (ISOMetricFineProfile), `UTS coarse` (UNC), `UTS fine` (UNF), `UTS extra fine` (UNEF), `ANSI pipes` (NPT), `ISO/BSP pipes` (BSP), `BSW whitworth` (BSW), `BSF whitworth fine` (BSF), `ISO tyre valves` (ISOTyre). |
| Size | Dropdown | *(first entry for selected standard)* | Nominal thread designation (e.g. `M6x1.0`, `#10`, `1/4`). Available options depend on the selected standard. Sets `ThreadSize`. |
| Head type | Dropdown | `None` | Cut type for the screw-head recess. `None`: plain hole, no recess. `Counterbore`: flat-bottomed cylindrical recess. `Countersink`: conical recess flush with the surface. `Counterdrill`: countersink with an additional flat cylindrical entry. Sets `HoleCutType`. |
| Depth type | Dropdown | `Dimension` | Controls how hole depth is determined. `Dimension`: use the explicit **Depth** value. `Through all`: hole penetrates the entire Body. Sets `DepthType`. |

### Hole cut / head dimensions

Visible when **Head type** is not `None`.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Custom head values | Checkbox | off | When ticked, enables manual editing of Head diameter and Head depth (and Countersink angle for countersink types). When unticked with a normed screw type selected, the fields show norm values and are read-only. Sets `HoleCutCustomValues`. |
| Head diameter | Length | *(from norm)* | Outer diameter of the counterbore or countersink. Must be larger than the hole diameter. Sets `HoleCutDiameter`. |
| Head depth | Length | *(from norm)* | For counterbore: depth of the flat-bottomed recess. For countersink: depth of the screw top below the surface. For counterdrill: depth of the cylindrical entry before the conical section. Sets `HoleCutDepth`. |
| Countersink angle | Angle | `90°` | Included angle of the countersink cone. Only active for `Countersink` and `Counterdrill` types. Sets `HoleCutCountersinkAngle`. Range: near 0° to near 180°. |

### Drill point

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Drill angle | Checkbox | off | When ticked, adds a conical drill-point tip at the bottom of blind holes, as produced by a standard twist drill. Sets `DrillPoint` to `Angled`. |
| Drill angle value | Angle | `118°` | Included angle of the drill-point cone. 118° is standard for HSS twist drills; 135° is common for carbide. Sets `DrillPointAngle`. Active only when `Drill angle` is ticked. |
| Include in depth | Checkbox | off | When ticked, the drill-point cone geometry is included within the nominal hole depth (the full-diameter portion is shortened). When unticked, the depth value specifies the cylindrical portion and the cone adds extra depth beyond it. Sets `DrillForDepth`. |

### Depth and taper

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Depth | Length | `25 mm` | Hole depth when **Depth type** is `Dimension`. Not active for `Through all`. Sets `Depth`. |
| Switch direction | Checkbox | off | Reverses the drill direction (hole goes the opposite way through the solid). Sets `Reversed`. |
| Tapered | Checkbox | off | Enables a conical wall angle on the hole. Sets `Tapered`. |
| Tapered angle | Angle | `90°` | Half-included wall angle. `90°` = straight hole. Less than 90°: hole narrows toward the bottom (taper inward). Greater than 90°: hole widens toward the bottom (taper outward). Sets `TaperedAngle`. Active only when `Tapered` is ticked. |

### Threading section

Visible when **Standard** is not `None`.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Hole type | Dropdown | `Clearance / Passthrough` | `Clearance / Passthrough`: plain hole sized for the screw to pass through (uses `ThreadFit` clearance). `Tap drill (to be threaded)`: sized for tapping, no 3-D thread geometry. `Modeled thread`: generates a real helical thread form. Sets `Threaded` and `ModelThread`. |
| Clearance | Dropdown | `Medium` (ISO) / `Normal` (UTS) | Clearance class for passthrough holes. ISO standards: `Fine`, `Medium`, `Coarse` (ISO 273). UTS standards: `Close`, `Normal`, `Loose` (ASME B18.2.8). Other: `Close`, `Normal`, `Wide`. Only visible when `Hole type` is `Clearance / Passthrough`. Sets `ThreadFit`. |
| Update thread view | Checkbox | off | When `Modeled thread` is active, recomputes the visible thread helix in real time as parameters change. Untick during editing to avoid slow recomputes. |
| Class | Dropdown | *(standard dependent)* | Thread tolerance class. ISO metric: `4G`, `4H`, `5G`, `5H`, `6G`, `6H`, `7G`, `7H`, `8G`, `8H`. UNC/UNF/UNEF: `1B`, `2B`, `3B`. BSW/BSF: `Medium`, `Normal`. Sets `ThreadClass`. Active only when threaded. |
| Direction | Radio | `Right hand` | Thread hand. `Right hand`: standard clockwise-tighten thread. `Left hand`: anti-clockwise. Sets `ThreadDirection`. |
| Thread Depth Type | Dropdown | `Hole depth` | How thread depth is calculated. `Hole depth`: thread extends the full hole depth. `Dimension`: use an explicit `Thread Depth` value. `Tapped (DIN76)`: adds DIN 76 thread run-out to the usable thread length. Sets `ThreadDepthType`. |
| Thread Depth | Length | `23.5 mm` | Explicit thread length. Only active when `Thread Depth Type` is `Dimension`. Sets `ThreadDepth`. |
| Custom Clearance | Checkbox + Length | off / `0 mm` | When ticked, overrides the `ThreadClass` clearance with a manually entered value (can be negative to tighten fit). Only visible when `Modeled thread` is active. Sets `UseCustomThreadClearance` and `CustomThreadClearance`. |

---

## Advanced usage

### Creating many holes in one operation

Place all hole centre circles (or points) in a single sketch and run the Hole
tool once. FreeCAD processes every circle or point in the profile and creates all
holes simultaneously. This is significantly faster to model and easier to update
parametrically than separate Pocket operations.

For a bolt-circle pattern, use the Sketcher's circular-pattern constraint to
place the centres, then apply Hole once. Changing the number of instances or the
pitch-circle diameter updates all holes.

### Modeled threads for FEA and presentation

Enable `Modeled thread` when you need a real helical thread form in the geometry
— for example, for FEA meshing or technical documentation renders. Note that
computing the helical geometry is slow for deep holes; untick **Update thread
view** while adjusting parameters and retick only to see the final result.

The clearance between the modeled thread crest and the surrounding material can
be adjusted per class (via `ThreadClass`) or overridden exactly with
`CustomThreadClearance`. A value of `0 mm` gives nominal geometry; a small
positive clearance simulates a real-world fit.

### Mixing head types with standard sizes

Selecting a screw standard automatically looks up the correct counterbore or
countersink geometry for that size from the embedded ISO 4762 (socket-head cap
screws) and ISO 10642 (flat-head screws) tables. If no norm entry exists for a
given size, FreeCAD computes a geometric approximation and forces
`Custom head values` to `true` so you can inspect and adjust the result.

To use a non-standard head size (e.g. a flanged hex bolt that does not appear
in the presets), select the nearest thread standard for the tap diameter, then
tick **Custom head values** and enter the required head diameter and depth.

### Tapered holes

The `Tapered` option produces a conical bore wall, which is useful for press-fit
pins, draft angles on moulded features, or matching a conical seat. The angle is
defined relative to the bore axis: 90° gives a straight cylinder, less than 90°
gives a truncated cone that narrows toward the bottom.

Tapered holes can be combined with all other options (countersink, drill point,
thread type) but the combination of a taper with `Modeled thread` is unusual and
the resulting geometry should be checked carefully.

---

## Common mistakes and pitfalls

!!! warning "Hole feature fails with 'No profile selected'"
    **Cause:** The sketch driving the Hole was not set as the `Profile` property,
    or the sketch contains no circles or points (and the default `Circles and arcs`
    profile mode found nothing to work with).  
    **Fix:** Select the Hole in the model tree and check that `Profile` points to
    the correct sketch. If the sketch uses only points, change **Base profile
    types** to `Points, circles and arcs` or `Points`.

!!! warning "All holes are drilled at the wrong depth or diameter"
    **Cause:** When `ThreadType` is not `None`, the `Diameter` field is driven by
    the selected thread size and is read-only. Setting it manually when the type
    is `None` and then switching to a standard silently overrides the value.  
    **Fix:** Set the thread standard first, then adjust size. If you need an
    exact non-standard diameter, keep `ThreadType` as `None` and enter the
    diameter directly.

!!! warning "Countersink/counterbore dimensions reset when size is changed"
    **Cause:** Changing **Size** resets `HoleCutCustomValues` to `false` and
    reloads the norm values from the embedded lookup table.  
    **Fix:** Make all custom head-dimension edits *after* locking in the size
    selection, then tick **Custom head values**.

!!! warning "Modeled thread is extremely slow to compute"
    **Cause:** Generating a real helical thread solid is OCCT-intensive and scales
    with thread depth. Long holes or very fine pitches can take tens of seconds.  
    **Fix:** Leave **Update thread view** unticked until you are satisfied with all
    other parameters, then tick it once to get the final geometry.

!!! warning "Hole does not appear even though the sketch is correct"
    **Cause:** The sketch is attached to a face but the hole direction points away
    from the solid (outside the material). This can happen if the sketch plane
    normal faces away from the Body.  
    **Fix:** Tick **Switch direction** in the task panel to reverse the drill
    direction, or flip the sketch's `MapReversed` property.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

# TODO: verify — tested against TestHole.py patterns in the FreeCAD source tree

doc = App.newDocument()

# Create a body and a base solid (10 × 10 × 10 mm box)
body = doc.addObject("PartDesign::Body", "Body")
box = doc.addObject("PartDesign::AdditiveBox", "Box")
box.Length = 10
box.Width = 10
box.Height = 10
body.addObject(box)
doc.recompute()

# Sketch on the top face (XY plane, reversed so normal points down into the solid)
sketch = doc.addObject("Sketcher::SketchObject", "HoleSketch")
sketch.AttachmentSupport = (doc.XY_Plane, [""])
sketch.MapMode = "FlatFace"
sketch.MapReversed = True
body.addObject(sketch)

# Add a circle at (-5, 5) — radius value is ignored by the Hole tool
sketch.addGeometry(
    Part.Circle(App.Vector(-5, 5, 0), App.Vector(0, 0, 1), 1), False
)
doc.recompute()

# Create the Hole feature
hole = doc.addObject("PartDesign::Hole", "Hole")
hole.Profile = sketch
body.addObject(hole)

# Plain 6 mm blind hole, 8 mm deep, flat drill point
hole.ThreadType = 0          # None — no thread standard
hole.Diameter = 6.0          # mm
hole.DepthType = 0           # 0 = Dimension, 1 = ThroughAll
hole.Depth = 8.0             # mm
hole.HoleCutType = 0         # 0 = None, 1 = Counterbore, 2 = Countersink, 3 = Counterdrill
hole.DrillPoint = 0          # 0 = Flat, 1 = Angled
hole.Tapered = False

doc.recompute()
print(f"Hole volume removed: {10**3 - hole.Shape.Volume:.4f} mm³")
```

### Setting up an M6 tapped hole

```python
# Continuing from the minimal example above (hole object already exists)

hole.ThreadType = 1          # ISOMetricProfile
hole.ThreadSize = 14         # M6x1.0 — index in the ISOMetricProfile list
hole.Threaded = True         # tap drill / threaded
hole.ModelThread = False     # no helical geometry (faster)
hole.ThreadClass = 5         # 6H  (index 5 in ThreadClass_ISOmetric_Enums)
hole.ThreadDirection = 0     # Right hand
hole.HoleCutType = 1         # Counterbore
hole.DepthType = 0           # Dimension
hole.Depth = 15.0
hole.DrillPoint = 1          # Angled
hole.DrillPointAngle = 118.0
hole.DrillForDepth = False
hole.ThreadDepthType = 0     # Hole depth (thread fills full depth)

doc.recompute()
```

### Feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The driving sketch containing circles or points. |
| `BaseProfileType` | `App::PropertyInteger` | `6` (OnCirclesArcs) | Bitmask: `1` = OnPoints, `2` = OnCircles, `4` = OnArcs. |
| `ThreadType` | `App::PropertyEnumeration` | `"None"` | Thread standard. Enum values: `"None"`, `"ISOMetricProfile"`, `"ISOMetricFineProfile"`, `"UNC"`, `"UNF"`, `"UNEF"`, `"NPT"`, `"BSP"`, `"BSW"`, `"BSF"`, `"ISOTyre"`. |
| `ThreadSize` | `App::PropertyEnumeration` | `"---"` | Nominal designation within the selected standard (e.g. `"M6x1.0"`). Enum list resets when `ThreadType` changes. |
| `Threaded` | `App::PropertyBool` | `False` | `True` if the hole is tapped (sets diameter to tap-drill size). |
| `ModelThread` | `App::PropertyBool` | `False` | `True` to generate a real helical thread solid. |
| `ThreadClass` | `App::PropertyEnumeration` | `"None"` | Tolerance class. Options depend on `ThreadType`. |
| `ThreadFit` | `App::PropertyEnumeration` | `"Medium"` | Clearance fit for passthrough holes. Options depend on `ThreadType`. |
| `ThreadDirection` | `App::PropertyEnumeration` | `"Right"` | `"Right"` or `"Left"`. |
| `ThreadDepthType` | `App::PropertyEnumeration` | `"Hole Depth"` | `"Hole Depth"`, `"Dimension"`, or `"Tapped (DIN76)"`. |
| `ThreadDepth` | `App::PropertyLength` | `23.5 mm` | Explicit thread depth used when `ThreadDepthType` is `"Dimension"`. |
| `ThreadPitch` | `App::PropertyLength` | *(from standard)* | Thread pitch; set automatically from the selected size. |
| `ThreadDiameter` | `App::PropertyLength` | `0 mm` | Read-only. Thread major diameter as derived from the selected standard and size. |
| `UseCustomThreadClearance` | `App::PropertyBool` | `False` | Override thread clearance with `CustomThreadClearance`. |
| `CustomThreadClearance` | `App::PropertyLength` | `0 mm` | Manual clearance offset. Active only when `UseCustomThreadClearance` is `True`. |
| `Diameter` | `App::PropertyLength` | `6 mm` | Hole diameter. Manually editable only when `ThreadType` is `"None"`; otherwise set by the standard. |
| `HoleCutType` | `App::PropertyEnumeration` | `"None"` | `"None"`, `"Counterbore"`, `"Countersink"`, `"Counterdrill"`. |
| `HoleCutCustomValues` | `App::PropertyBool` | `False` | Enable manual override of head dimensions. |
| `HoleCutDiameter` | `App::PropertyLength` | `0 mm` | Outer diameter of the head recess. |
| `HoleCutDepth` | `App::PropertyLength` | `0 mm` | Depth of the head recess. |
| `HoleCutCountersinkAngle` | `App::PropertyAngle` | `90°` | Included angle of the countersink or counterdrill cone. |
| `DepthType` | `App::PropertyEnumeration` | `"Dimension"` | `"Dimension"` or `"ThroughAll"`. |
| `Depth` | `App::PropertyLength` | `25 mm` | Hole depth when `DepthType` is `"Dimension"`. |
| `DrillPoint` | `App::PropertyEnumeration` | `"Angled"` | `"Flat"` or `"Angled"`. |
| `DrillPointAngle` | `App::PropertyAngle` | `118°` | Included angle of the drill-point cone. Active when `DrillPoint` is `"Angled"`. |
| `DrillForDepth` | `App::PropertyBool` | `False` | Include the drill-point cone within the nominal `Depth`. |
| `Tapered` | `App::PropertyBool` | `False` | Enable a conical wall angle. |
| `TaperedAngle` | `App::PropertyAngle` | `90°` | Wall cone half-angle. `90°` = straight; `< 90°` = narrows; `> 90°` = widens. |

---

## See also

- [Pocket](pocket.md) — general subtractive extrusion for non-circular cutouts
- [Revolution](revolution.md) — remove material by revolving a profile, useful for conical bores
- [Subtractive Pipe](subtractive-pipe.md) — remove material along a swept path
- [Pad](pad.md) — add material by extrusion (the additive counterpart to Pocket)
