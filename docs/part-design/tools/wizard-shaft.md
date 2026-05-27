# Shaft Design Wizard

> **In one sentence:** Build a multi-segment stepped shaft solid from a table of segment lengths and diameters, with optional FEM constraints for bearing reaction and load analysis, without manually stacking Revolution and sketch operations.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Shaft design wizard...  
**Toolbar:** none · **Shortcut:** none

The Shaft Design Wizard is a Python-based dialog that generates a PartDesign revolution solid from a tabular description of shaft segments. Each column in the table represents one shaft segment; each row sets a geometric or constraint property of that segment. As you fill in the table, the 3D model updates live.

The wizard also integrates with the FEM workbench: each segment can carry a constraint (Fixed end, applied Force, Bearing, Gear load, or Pulley load). When constraints are defined, the wizard solves the static equilibrium equations and can display diagrams of section forces, bending moments, deflection, and stress along the shaft axis.

The wizard creates standard PartDesign objects: a `Sketcher::SketchObject` (the half-profile of the shaft) and a `PartDesign::Revolution` feature (the revolved solid). Both appear in the model tree and behave as regular PartDesign objects after the wizard is closed.

---

## Intuition

Designing a stepped shaft normally means sketching a stepped half-profile in Sketcher, adding dimensional constraints for every diameter and length, then revolving it. For a shaft with many steps, that is dozens of constraints and careful geometry management.

The Shaft Wizard replaces the sketch work with a spreadsheet-like table: type the length and diameter of each section into a column, and FreeCAD constructs the constrained half-profile sketch and revolution automatically. The sketch stays live — you can still open and edit it after closing the wizard.

The integrated FEM-light solver adds structural insight without a full simulation setup: define where the shaft is fixed and where loads are applied, and the wizard draws shear-force and bending-moment diagrams that let you check whether the shaft design is feasible before committing to a final material and manufacturing process.

---

## When to use it

- You are designing a stepped transmission shaft and want to iterate quickly on segment lengths and diameters.
- You want a quick structural sanity check (shear force, bending moment, deflection, stress) without setting up a full FEM simulation.
- You need a revolution solid from a tabular specification, not from a hand-drawn sketch.
- You are teaching or learning PartDesign and want a guided introduction to revolved profiles.

---

## When NOT to use it

- **The shaft has non-circular cross-sections (keyways, flats, splines)** — the wizard only generates circular and hollow-circular sections. Add keyway Pockets and spline cuts as separate PartDesign features after the wizard closes.
- **You need precise control over sketch constraints or geometry** — after the wizard generates the sketch and revolution, you can edit the sketch directly, but the wizard itself does not expose all sketcher constraint types.
- **The shaft is conical or has varying diameter within one segment** — the wizard only supports constant-diameter (cylindrical) and constant-inner-diameter (hollow cylindrical) segments. Use a loft or a manually-sketched revolution for tapered shafts.
- **You need a full FEM simulation** — the built-in equilibrium solver is a simplified Euler–Bernoulli beam model. For accurate stress analysis, export the revolution solid to the FEM workbench and use full 3D simulation.

---

## Step-by-step walkthrough

**Prerequisites:** An active document. The wizard can open from a blank document or one that already has a Body.

1. **Start the wizard.** Go to **Part Design → Shaft design wizard...**. The Shaft Wizard dialog appears. If no document is open, the wizard creates a new one and activates the PartDesign workbench automatically.

2. **Read the default table.** The table opens with two default sections pre-filled:
    - Section 1: Length = 40 mm, Diameter = 50 mm, Constraint = Fixed
    - Section 2: Length = 80 mm, Diameter = 60 mm, Constraint = Force

3. **Understand the table layout.** Sections are columns. Rows are:
    | Row | Description |
    |-----|-------------|
    | Length [mm] | Axial length of the segment |
    | Diameter [mm] | Outer diameter of the segment |
    | Inner diameter [mm] | Inner diameter (0 for solid, >0 for hollow tube) |
    | Constraint type | Load/boundary condition at this segment |
    | Start edge type | Chamfer or Fillet at the left edge |
    | Start edge size | Size of the start edge feature |
    | End edge type | Chamfer or Fillet at the right edge |
    | End edge size | Size of the end edge feature |

4. **Add a section.** Right-click on any column header or anywhere in the table and choose **Add column**. A new section is appended. The wizard makes an intelligent guess at the diameter of the next section (increasing or decreasing by 5 mm from the previous).

5. **Edit a segment.** Click any numeric cell to edit its value. Changes trigger an immediate live update of the 3D revolution solid.

6. **Set constraint types.** For each segment, the **Constraint type** row shows a dropdown with these options:
    - **None** — no constraint (free segment).
    - **Fixed** — clamped end condition (zero translation and rotation). Must be at the first or last segment.
    - **Force** — a point force applied at the midpoint of the segment. The force vector and magnitude are set by editing the `Fem::ConstraintForce` object.
    - **Bearing** — a bearing support (zero radial translation, configurable axial freedom). Creates a `Fem::ConstraintBearing` object.
    - **Gear** — gear load (radial force and torque). Creates a `Fem::ConstraintGear` object.
    - **Pulley** — belt/pulley load. Creates a `Fem::ConstraintPulley` object.

7. **Edit a constraint.** Right-click the constraint dropdown cell and choose **Edit constraint**. The FEM constraint task panel opens inline. Set the force magnitude, direction, bearing parameters, gear diameter, or belt tension as required. Click OK in the constraint panel to return to the wizard.

8. **Set edge types (optional).** The **Start edge type** and **End edge type** rows accept **Chamfer** or **Fillet** at each segment transition. Note: as of FreeCAD 1.1, edge feature application in the wizard requires robust topological references that may not be fully implemented. The size fields are available but the 3D dress-up may not apply automatically — add chamfers and fillets manually as separate PartDesign features after closing the wizard.

9. **View diagrams.** Once constraints are configured, the wizard solves the equilibrium and enables the diagram buttons at the bottom of the dialog. Click any button to display the corresponding plot (requires the FreeCAD **Plot** add-on to be installed):
    - Row 1 — All [x], All [y], All [z]: all diagrams for a given axis.
    - N [x] — Normal/axial force.
    - Q [y], Q [z] — Shear force in y and z.
    - Mt [x] — Torque.
    - Mb [z], Mb [y] — Bending moment.
    - w [y], w [z] — Deflection / bending line (requires two boundary conditions).
    - sigma [x..z] — Normal and shear stresses.
    - tau [x], sigmab [z], sigmab [y] — Torsional and bending stresses.

10. **Click OK** to close the wizard. The underlying sketch and revolution solid remain in the document as normal PartDesign features.

!!! tip
    The wizard axis convention is: the shaft axis is always the **X axis**. The sketch is created on the XY plane with the shaft extending in the positive X direction. Diameters are measured as full diameters (not radii) in the table, but the sketch stores half the diameter as the radius constraint.

!!! tip
    After closing the wizard, the generated sketch (`SketchShaft`) is hidden. To inspect or modify it, select it in the model tree and press **Space** to make it visible, then double-click to open it in Sketcher.

---

## Parameters

### Table columns (per segment)

| Row | Type | Default | Description |
|-----|------|---------|-------------|
| Length [mm] | Double (≥ 0) | 20.0 (guessed) | Axial length of this shaft segment. |
| Diameter [mm] | Double (≥ 0) | Guessed from previous segment | Outer diameter of the segment. |
| Inner diameter [mm] | Double (≥ 0) | 0.0 | Inner diameter (0 = solid, >0 = hollow). Currently a global value shared across all segments. |
| Constraint type | Enumeration | None | Load or boundary condition at this segment: None, Fixed, Force, Bearing, Gear, Pulley. |
| Start edge type | Enumeration | None | Chamfer or Fillet at the left (start) edge of the segment. |
| Start edge size | Double | 1.0 | Size of the start edge feature (chamfer width or fillet radius). |
| End edge type | Enumeration | None | Chamfer or Fillet at the right (end) edge. |
| End edge size | Double | 1.0 | Size of the end edge feature. |

### Structural solver assumptions

The equilibrium solver uses the following assumptions:

| Assumption | Value |
|------------|-------|
| Material | Steel (Young's modulus = 210 000 N/mm² = 2.1 × 10¹² N/m²) |
| Cross-section | Circular or hollow-circular (per segment) |
| Beam theory | Euler–Bernoulli |
| Fixed supports | Must be at first or last segment only |
| Maximum independent reaction unknowns per axis | 2 |

---

## Advanced usage

### Adding more segments

Right-click anywhere in the table header area and choose **Add column**. The wizard appends a new section to the right with guessed length and diameter values. There is no hard limit on the number of sections, but the equilibrium solver becomes indeterminate if you add more than two independent reaction-force unknowns per axis (statically indeterminate shaft).

### Hollow shafts

Set a non-zero value in the **Inner diameter [mm]** row. The inner diameter applies globally to the whole shaft (the generated sketch uses a single inner-radius constraint shared across all segments). Per-segment inner diameters are not yet supported; for varying bore diameters, add bore Pocket features to the revolution solid after closing the wizard.

### Editing the generated sketch

The wizard creates `SketchShaft` with explicit horizontal/vertical constraints and distance constraints for each segment length and radius. After closing the wizard, you can:

- Open `SketchShaft` in Sketcher to change any dimension.
- Add new geometry (a keyway slot, for example) without breaking the wizard-generated constraints.
- The revolution feature (`RevolutionShaft`) automatically updates when the sketch changes.

### Using the diagram buttons

Diagram buttons are enabled only when the equilibrium solver has a solution. If some buttons remain greyed out, check that:

1. The shaft has at least two independent boundary conditions (e.g. one Fixed + one Bearing, or two Bearings).
2. `numpy` is installed in your FreeCAD Python environment (required for the linear algebra solver).
3. The `FreeCAD Plot` add-on is installed (required for rendering diagrams).

The wizard prints equilibrium equations to the **Report View** panel — inspect these if the solver reports "Matrix is singular, no solution possible."

### Exporting to FEM

After closing the wizard, the revolution solid is a standard PartDesign Body. Open the FEM workbench and use **FEM → Analysis → New Analysis** to attach the body to a proper FEM mesh and solver. The FEM constraints created by the wizard (Force, Bearing, Gear, Pulley) are separate document objects and may need to be reassigned to the correct FEM mesh faces.

---

## Common mistakes and pitfalls

!!! warning "Fixed constraint must be at beginning or end of shaft"
    **Cause:** A segment with **Constraint type = Fixed** was placed in the middle of the shaft, which violates the beam model's boundary conditions.  
    **Fix:** Move the Fixed constraint to the first or last segment only. The equilibrium solver prints a message to the Report View if this rule is violated.

!!! warning "numpy not installed — no equilibrium solution"
    **Cause:** The Python `numpy` library is not available in FreeCAD's Python environment.  
    **Fix:** Install numpy. On most platforms, FreeCAD ships with numpy, but conda or custom builds may omit it. Run `import numpy` in the FreeCAD Python console to check. If it fails, install it via your system package manager or `pip install numpy`.

!!! warning "Diagram buttons remain greyed out after setting constraints"
    **Cause:** Either the system is statically indeterminate (more than two independent reaction unknowns per axis), the equilibrium matrix is singular, or Plot add-on is not installed.  
    **Fix:** Verify that the shaft has exactly two independent boundary conditions per axis. Install the **FreeCAD Plot** add-on from the Add-on Manager (Tools → Add-on Manager). Check the Report View for equilibrium equations and error messages.

!!! warning "Inner diameter change only affects the first segment"
    **Cause:** The generated sketch uses a single inner-radius constraint (constraint index 1) that controls all segments' inner radii simultaneously.  
    **Fix:** This is a known limitation of the current implementation. For varying bore diameters, set `Inner diameter = 0` in the wizard and add individual bore Pockets to the revolution solid afterwards.

!!! warning "Edge chamfers/fillets defined in the table do not appear in the 3D model"
    **Cause:** The edge feature implementation in the wizard requires robust topological references that are noted as incomplete in the source code.  
    **Fix:** Leave edge types as **None** in the table. After closing the wizard, add [Chamfer](chamfer.md) and [Fillet](fillet.md) features manually as separate PartDesign dress-up operations on the `RevolutionShaft` feature.

---

## Python API

The Shaft Wizard is primarily a GUI tool. There is no standalone Python function to invoke it non-interactively. You can, however, start it from the FreeCAD Python console:

```python
# Start the Shaft Design Wizard from the Python console
import FreeCADGui as Gui
Gui.runCommand('PartDesign_WizardShaft')
```

To create a shaft programmatically without the wizard UI, use the underlying sketch and revolution objects directly:

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Build the half-profile sketch manually (equivalent to what WizardShaft generates)
sketch = doc.addObject("Sketcher::SketchObject", "SketchShaft")
sketch.Placement = App.Placement(App.Vector(0, 0, 0), App.Rotation(0, 0, 0, 1))

# Segment 1: length=40, diameter=50 (radius=25)
# Segment 2: length=80, diameter=60 (radius=30)
# The wizard generates a half-profile above the X axis; X axis is the shaft axis.
# Simplified manual equivalent (no automatic sketch constraints):
sketch.addGeometry(Part.LineSegment(App.Vector(0, 0, 0), App.Vector(120, 0, 0)))   # centre line
sketch.addGeometry(Part.LineSegment(App.Vector(0, 0, 0), App.Vector(0, 25, 0)))     # start vertical
sketch.addGeometry(Part.LineSegment(App.Vector(0, 25, 0), App.Vector(40, 25, 0)))   # seg 1 horizontal
sketch.addGeometry(Part.LineSegment(App.Vector(40, 25, 0), App.Vector(40, 30, 0)))  # step vertical
sketch.addGeometry(Part.LineSegment(App.Vector(40, 30, 0), App.Vector(120, 30, 0))) # seg 2 horizontal
sketch.addGeometry(Part.LineSegment(App.Vector(120, 30, 0), App.Vector(120, 0, 0))) # end vertical
doc.recompute()

# Revolve around the X axis (H_Axis of the sketch)
revolution = doc.addObject("PartDesign::Revolution", "RevolutionShaft")
revolution.Profile = sketch
revolution.ReferenceAxis = (sketch, ["H_Axis"])
revolution.Angle = 360.0
doc.recompute()

print(f"Shaft volume: {revolution.Shape.Volume:.1f} mm³")
```

### Key objects created by the wizard

| Object | Type | Description |
|--------|------|-------------|
| `SketchShaft` | `Sketcher::SketchObject` | Half-profile of the stepped shaft. Contains constrained geometry for all segments. |
| `RevolutionShaft` | `PartDesign::Revolution` | 360° revolution of `SketchShaft` around its H_Axis. Produces the solid shaft body. |
| `ShaftConstraintFixed` | `Fem::ConstraintFixed` | Fixed-end boundary condition (created when Constraint type = Fixed). |
| `ShaftConstraintForce` | `Fem::ConstraintForce` | Applied force (created when Constraint type = Force). |
| `ShaftConstraintBearing` | `Fem::ConstraintBearing` | Bearing support (created when Constraint type = Bearing). |
| `ShaftConstraintGear` | `Fem::ConstraintGear` | Gear load (created when Constraint type = Gear). |
| `ShaftConstraintPulley` | `Fem::ConstraintPulley` | Pulley load (created when Constraint type = Pulley). |

---

## See also

- [Pad](pad.md) — alternative for extruded profiles
- [Revolution](groove.md) — manual revolution tool for custom profiles
- [Groove](groove.md) — subtractive revolution for bores and keyways
- [Fillet](fillet.md) — add fillets at step transitions after closing the wizard
- [Chamfer](chamfer.md) — add chamfers at shaft ends after closing the wizard
- [Pocket](pocket.md) — add keyways, flats, or cross-holes to the revolution solid
