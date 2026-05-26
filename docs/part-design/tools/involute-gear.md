# Involute Gear

> **In one sentence:** Generate a fully constrained 2-D sketch profile of an
> involute gear tooth form, ready to be extruded into a solid gear.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Involute Gear  
**Toolbar:** none (menu only) · **Shortcut:** none

Involute Gear creates a `Part::Part2DObjectPython` feature — a parametric 2-D
wire — in the shape of a complete involute gear outline. The result is a closed
sketch-like profile placed inside the active Body (or Part). It is not a solid
by itself; you must follow it with a [Pad](pad.md) or [Revolution](revolution.md)
to obtain a 3-D gear. All tooth geometry is driven by a small set of standard
gear parameters, so changing the module or tooth count immediately regenerates the
correct profile.

---

## Intuition

An involute gear tooth has a specific curved flank shape — the *involute* — that
guarantees smooth rolling contact between meshing gears regardless of small
centre-distance errors. Drawing that curve by hand with splines would be tedious
and nearly impossible to get right. This tool calculates the exact involute curve
from the engineering parameters you already know (module, number of teeth, pressure
angle) and hands you a finished, dimensionally correct profile.

Think of it as a gear calculator that outputs geometry rather than numbers. You
specify the gear's standard parameters, and the tool fills in every curve — the
involute flanks, the root fillets, and the tip arc — then assembles them into one
closed wire that is ready to extrude.

The profile lives in the XY plane by default. Once you Pad it, you have a spur
gear body. If you want a helical gear, apply a [Draft](draft.md) angle or use an
[Additive Helix](additive-helix.md) as the sweep path.

---

## When to use it

- You need a standard spur gear profile and do not want to draw involute curves
  manually.
- You are building a parametric gear train and want the tooth geometry to update
  automatically when you change the module or tooth count.
- You need either an external (conventional, outward-pointing teeth) or internal
  (ring gear, inward-pointing teeth) gear profile.
- You want to use profile-shift (rack offset) to correct for undercut on
  low-tooth-count gears or to adjust the meshing centre distance.

---

## When NOT to use it

- **You need a 3-D bevel, helical, or worm gear** — this tool only produces a
  2-D spur-gear cross-section. Create the profile here and then use an
  [Additive Pipe](additive-pipe.md) with a helical spine for helical gears, or
  look for dedicated add-ons (e.g. the FCGear workbench) for more gear types.
- **You need a sprocket for a roller chain** — use [Sprocket](sprocket.md), which
  uses chain-standard tooth geometry rather than involute curves.
- **You need to mesh two gears with animated motion** — this tool produces
  geometry only; simulation and animation require the Assembly workbench or an
  external tool.

---

## Step-by-step walkthrough

**Prerequisites:** An active document. If you want the gear inside a Body for
later Part Design operations, activate or create a Body first.

1. **Open Involute Gear** via Part Design → Involute Gear.  
   The **Involute Parameter** task panel opens and a gear profile appears
   immediately in the viewport using the default parameters.

2. **Set Number of teeth.** Enter the integer tooth count in the *Number of teeth*
   spin box. The default is **26**. Typical spur gears range from 12 to 200 teeth.

    !!! warning "Low tooth counts cause undercut"
        Gears with fewer than roughly 17 teeth (at the standard 20° pressure angle
        and zero profile shift) will have undercut at the root, weakening the tooth.
        Either increase the number of teeth, increase the pressure angle, or apply a
        positive **Profile shift coefficient** to eliminate the undercut.

3. **Set Module.** Enter the gear module (tooth size) in the *Module* field. The
   default is **2.5 mm**. The module equals the pitch diameter divided by the tooth
   count; all meshing gears in a train must share the same module.

4. **Set Pressure angle.** Enter the standard pressure angle. The default is
   **20°**, which is the ISO and AGMA standard for most spur gears. Older designs
   sometimes use 14.5° or 25°.

5. **Choose gear type.** Set *External gear* to `True` for a conventional gear
   with outward-pointing teeth (default), or `False` for a ring gear (internal
   gear) with inward-pointing teeth.

6. **Adjust advanced coefficients** if needed:
   - *Addendum coefficient* — tooth height above the pitch circle, normalised by
     the module. Default **1.0** (external) or **0.6** (internal).
   - *Dedendum coefficient* — tooth depth below the pitch circle, normalised by
     the module. Default **1.25**.
   - *Root fillet coefficient* — fillet radius at the tooth root, normalised by
     the module. Default **0.38**.
   - *Profile shift coefficient* — outward shift of the reference profile,
     normalised by the module. Default **0.0** (no shift).

7. **Set High precision** to `True` (default) to use two 3-control-point Bézier
   curves per involute flank (more accurate). Set to `False` to use a single
   4-control-point curve (faster for large assemblies).

8. **Click OK.** The gear wire is created and added to the Body (or the active
   Part if no Body is active).

9. **Pad or revolve the wire.** Select the gear feature, then apply
   [Pad](pad.md) with the desired gear face width to produce a solid spur gear.

!!! tip
    To edit the gear after creation, double-click the gear object in the model
    tree. The task panel reopens with the current values, and any changes
    immediately update the profile in the viewport.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Number of teeth | Integer | `26` | Total number of gear teeth. Minimum is 3 (UI enforces this). |
| Module | Length | `2.5 mm` | Tooth size parameter. Equals pitch diameter ÷ number of teeth. All gears in a mesh must share the same module. |
| Pressure angle | Angle | `20°` | Angle between the tooth flank normal and the pitch-circle tangent. Standard values are 14.5°, 20°, or 25°. |
| High precision | Dropdown (`True`/`False`) | `True` | `True`: each involute flank is approximated by two 3-control-point Bézier curves (more accurate). `False`: one 4-control-point curve (faster). |
| External gear | Dropdown (`True`/`False`) | `True` | `True`: external gear (teeth point outwards). `False`: internal/ring gear (teeth point inwards). |
| Addendum coefficient | Float | `1.0` (external), `0.6` (internal) | Tooth height above the pitch circle, divided by the module. Standard value is 1.0. |
| Dedendum coefficient | Float | `1.25` | Tooth depth below the pitch circle, divided by the module. Standard value is 1.25 (includes clearance). |
| Root fillet coefficient | Float | `0.38` | Radius of the fillet at the tooth root, divided by the module. |
| Profile shift coefficient | Float | `0.0` | Outward displacement of the reference profile from the gear centre, divided by the module. Positive values reduce undercut and increase tooth thickness. |

---

## Advanced usage

### Profile shift to eliminate undercut

For gears with fewer than about 17 teeth at a 20° pressure angle, the tooth root
may be undercut, weakening the base of the tooth. A positive profile shift moves
the cutter outward, effectively thickening the root. A shift coefficient of
`x = 1 - z / 17` (where `z` is the tooth count) is a common starting point. The
meshing gear should receive an equal-and-opposite shift to maintain the correct
centre distance.

### Meshing a pair of gears

Two gears mesh correctly when they share the same module and pressure angle. For
zero-shift gears the centre distance is `(z1 + z2) * m / 2`. When using profile
shift, the operating centre distance changes; consult a gear design reference for
the corrected formula.

### Internal gear (ring gear)

Setting *External gear* to `False` generates a ring gear profile. The default
addendum coefficient changes to 0.6, which is typical for internal gears. The
resulting wire represents the inner bore profile with inward-facing teeth. Pad
this wire to produce a ring gear body, then use Boolean Cut to create a housing
or planet-carrier clearance if needed.

---

## Common mistakes and pitfalls

!!! warning "Profile is an open wire or wire builder error"
    **Cause:** Extreme combinations of parameters (very few teeth with large
    addendum or large profile shift) can push the tip circle inside the base
    circle, making a valid involute impossible.  
    **Fix:** Reduce the addendum coefficient, add a positive profile shift, or
    increase the tooth count.

!!! warning "Pad fails on the gear profile"
    **Cause:** The gear wire is a `Part::Part2DObjectPython`, not a `Sketcher::SketchObject`.
    Some Part Design operations require a Sketcher-based profile.  
    **Fix:** If Pad reports an error, try using Part → Extrude instead, or
    convert the wire to a sketch by mapping it to a face.
    <!-- TODO: verify whether Pad accepts Part2DObjectPython directly -->

!!! warning "Two gears do not mesh smoothly"
    **Cause:** The gears have different modules or different pressure angles.  
    **Fix:** Ensure both gears use identical Module and Pressure angle values.
    Pitch diameter = Module × NumberOfTeeth; verify that the sum of pitch radii
    equals the actual centre distance in the assembly.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import FreeCADGui as Gui

doc = App.newDocument()

# Load the InvoluteGear module and create a gear feature
Gui.activateWorkbench("PartDesignWorkbench")
import InvoluteGearFeature
gear = InvoluteGearFeature.makeInvoluteGear("InvoluteGear")

# Adjust parameters
gear.NumberOfTeeth = 20
gear.Modules = "3 mm"
gear.PressureAngle = "20 deg"
gear.ExternalGear = True
gear.HighPrecision = True
gear.AddendumCoefficient = 1.0
gear.DedendumCoefficient = 1.25
gear.RootFilletCoefficient = 0.38
gear.ProfileShiftCoefficient = 0.0

doc.recompute()
print(f"Gear wire created: {gear.Shape.Length:.2f} mm perimeter")
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `NumberOfTeeth` | `App::PropertyInteger` | Number of gear teeth. |
| `Modules` | `App::PropertyLength` | Gear module (tooth size). |
| `PressureAngle` | `App::PropertyAngle` | Pressure angle of the tooth flank. |
| `HighPrecision` | `App::PropertyBool` | Use two 3-CP Béziers per flank (`True`) or one 4-CP Bézier (`False`). |
| `ExternalGear` | `App::PropertyBool` | `True` for external gear, `False` for internal/ring gear. |
| `AddendumCoefficient` | `App::PropertyFloat` | Addendum height divided by module. |
| `DedendumCoefficient` | `App::PropertyFloat` | Dedendum depth divided by module. |
| `RootFilletCoefficient` | `App::PropertyFloat` | Root fillet radius divided by module. |
| `ProfileShiftCoefficient` | `App::PropertyFloat` | Reference profile shift divided by module. |

---

## See also

- [Sprocket](sprocket.md) — roller-chain sprocket profile (non-involute)
- [Pad](pad.md) — extrude the gear profile into a solid spur gear
- [Additive Pipe](additive-pipe.md) — sweep the profile along a helix for helical gears
- [Revolution](revolution.md) — revolve a profile around an axis
