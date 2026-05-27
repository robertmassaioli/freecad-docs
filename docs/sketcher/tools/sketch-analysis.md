# Sketch Analysis Tools

> **In one sentence:** A collection of diagnostic and maintenance tools for
> inspecting constraint health, selecting specific categories of geometry, and
> bulk-deleting geometry or constraints from a sketch.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher analysis / Sketch  
**Toolbar:** Sketcher Analysis / Sketch · **Shortcut:** various

These tools help you understand what is happening in a complex or broken sketch
without manually searching through every element.

| Tool | Description |
|------|-------------|
| Select Associated Constraints | Select all constraints connected to the currently selected geometry |
| Select Origin | Select the sketch origin point |
| Select Vertical Axis | Select the sketch Y-axis construction line |
| Select Horizontal Axis | Select the sketch X-axis construction line |
| Select Redundant Constraints | Select all constraints the solver has flagged as redundant |
| Select Conflicting Constraints | Select all constraints causing over-constraint conflicts |
| Select Malformed Constraints | Select all degenerate or broken constraints |
| Select Partially Redundant Constraints | Select constraints that partially overlap with others |
| Select Associated Geometry | Select all geometry connected to selected constraints |
| Select Under-Constrained Elements | Select all geometry with remaining degrees of freedom |
| Toggle Internal Geometry | Show/hide internal geometry of ellipses, B-Splines, etc. |
| Delete All Geometry | Remove every geometry element from the sketch |
| Delete All Constraints | Remove every constraint from the sketch |
| Remove Axes Alignment | Delete any horizontal/vertical constraints imposed by automatic axes alignment |

---

## Intuition

A complex sketch with 50+ elements and 100+ constraints can become hard to navigate
visually. These selection tools act like a filter: "show me only what is broken"
or "show me everything connected to this constraint." Once the relevant elements
are selected, you can inspect, delete, or replace them efficiently.

The bulk-delete tools (Delete All Geometry, Delete All Constraints) are "nuclear
options" for restarting a sketch or clearing imported noise without leaving the
sketch edit mode.

---

## When to use them

- **Select Redundant / Conflicting Constraints:** Your sketch turned red and you
  cannot find which constraints are in conflict. Use these tools to highlight them,
  then delete the duplicates.

- **Select Under-Constrained Elements:** You cannot close the sketch because some
  elements are still white. Use this tool to highlight every remaining under-constrained
  element and systematically constrain them.

- **Toggle Internal Geometry:** You drew an ellipse but cannot see its focus points
  to add constraints to them. Toggle internal geometry to show them.

- **Select Associated Constraints:** You want to delete a line but need to know
  which constraints reference it first.

- **Delete All Geometry / Constraints:** You imported a DXF that is a mess and
  want to start fresh, or you want to completely repopulate the constraint set.

---

## Step-by-step walkthrough

### Finding and fixing redundant constraints

1. **Enter sketch edit mode** (double-click the sketch).
2. Notice elements turning red — the solver reports an over-constraint.
3. **Sketch → Select Redundant Constraints.** The redundant constraints are
   highlighted (selected) in the viewport.
4. **Press ++delete++** to remove the selected redundant constraints.
5. The sketch returns to a valid (non-red) state.

### Finding under-constrained elements

1. In sketch edit mode, the DoF counter shows a positive number (e.g. "3 DoF").
2. **Sketch → Select Under-Constrained Elements.** All white (unconstrained)
   geometry is now selected.
3. Apply the appropriate constraints to the selected elements.
4. Repeat until DoF = 0 and all elements are green.

### Toggling internal geometry

1. **Select an ellipse or B-Spline** in the sketch.
2. **Sketch → Toggle Internal Geometry** (or right-click → Toggle internal geometry).
3. The focus points, weight circles, and control polygon appear/disappear.
4. With internal geometry visible, select focus points to add constraints.

---

## Parameters

Most analysis tools have no parameters — they are one-click actions. Exceptions:

**Delete All Geometry / Delete All Constraints:** A confirmation dialog appears to
prevent accidental bulk deletion.

---

## Advanced usage

### Remove Axes Alignment

When you draw a rectangle or import geometry, FreeCAD may automatically add
Horizontal and Vertical constraints to enforce axis alignment. If you want to
rotate the geometry afterwards, these constraints prevent it. Use **Remove Axes
Alignment** to delete them in bulk and then apply an [Angle](constraint-angle.md)
constraint instead.

### Select Associated Geometry and Constraints (workflow)

These two tools work together for constraint archaeology:

1. Select a problematic constraint → **Select Associated Geometry** → see which
   edges it references.
2. Select a problematic edge → **Select Associated Constraints** → see which
   constraints reference it.

Use this pair when trying to understand why a change to one element breaks something
unexpected elsewhere in the sketch.

---

## Python API

### Select under-constrained elements (inspection)

```python
import FreeCAD as App

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Check degrees of freedom
dof = sketch.getDOF()
print(f"Remaining DoF: {dof}")

# List which constraints are redundant (solver result)
redundant = sketch.getRedundancies()
print(f"Redundant constraint indices: {redundant}")

# List conflicting constraints
conflicting = sketch.getConflicts()
print(f"Conflicting constraint indices: {conflicting}")
```

### Delete all constraints

```python
# Remove all constraints from the sketch
sketch.deleteAllConstraints()
doc.recompute()
```

### Toggle construction mode (internal geometry)

```python
import Sketcher

# Toggle a geometry element between regular and construction mode
sketch.toggleConstruction(geoIndex)
doc.recompute()
```

---

## See also

- [Validate Sketch](validate-sketch.md) — automated scan for missing coincident constraints
- [Coincident](constraint-coincident.md) — manually add missing connections
- [Toggle Driving / Active](toggle-constraint.md) — disable specific constraints non-destructively
- [Dimension](dimension.md) — add missing dimensional constraints to under-constrained elements
