# Toggle Driving Constraint / Toggle Active Constraint

> **In one sentence:** Flip a dimensional constraint between driving (controls
> geometry) and reference (reads geometry) mode (Toggle Driving), or completely
> enable or disable a constraint without deleting it (Toggle Active).

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Toggle Driving / Toggle Active  
**Toolbar:** Sketcher Constraints · **Shortcut:** `K, X` (Toggle Driving) · `K, Z` (Toggle Active)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Toggle Driving Constraint | `K, X` | Swap a dimensional constraint between driving (blue/coloured) and reference (grey/blue) mode |
| Toggle Active Constraint | `K, Z` | Enable or disable a constraint; disabled constraints appear greyed out and the solver ignores them |

---

## Intuition

### Toggle Driving

Think of a ruler with a locking slider: in *driving* mode the slider controls the
dimension and forces geometry to move; in *reference* mode the slider reads freely
as geometry moves but does not constrain it. Toggle Driving flips the lock on and
off.

### Toggle Active

Think of a toggle switch on a circuit: flipping it off cuts power without removing
the wire. Toggle Active cuts the constraint out of the solver temporarily — useful
for debugging or experimenting with different constraint sets.

---

## When to use them

### Toggle Driving

- A constraint is over-constrained (shown in red) because it conflicts with another;
  toggle it to reference to break the conflict while keeping the measurement visible.
- You finished a sketch and want to add inspection dimensions (reference) without
  disturbing the fully constrained geometry.
- You want to visually mark a derived measurement (e.g. the diagonal of a rectangle)
  without adding a constraint.

### Toggle Active

- You want to temporarily remove a constraint to see how the sketch behaves without
  it (for debugging).
- You have alternative constraint configurations and want to switch between them
  non-destructively.
- You want to study how geometry would behave if a constraint were removed, without
  deleting it permanently.

---

## Step-by-step walkthrough

### Toggle Driving

1. **Select a dimensional constraint** (a distance, radius, angle, or lock
   constraint; geometric constraints like Parallel cannot be toggled to reference).
2. **Press `K, X`** or right-click the constraint label → **Toggle Driving**.
3. The constraint switches mode:
    - Driving → Reference: label turns blue (or grey depending on theme),
      `(ref)` suffix appears. The solver no longer uses this value to control
      geometry.
    - Reference → Driving: label returns to its normal colour. The solver
      re-imposes the value.

### Toggle Active

1. **Select any constraint** (dimensional or geometric).
2. **Press `K, Z`** or right-click → **Toggle Active Constraint**.
3. The constraint is greyed out and the solver ignores it. Press `K, Z` again
   to re-enable it.

---

## Parameters

No numeric parameters. Both tools are pure toggles.

---

## Common mistakes and pitfalls

!!! warning "Toggling a constraint to reference does not remove it from the tree"
    **Cause:** The constraint still exists in the model; it just has no effect on
    geometry. If you want to delete it entirely, select it and press ++delete++.  
    **Fix:** For cleanup, delete reference constraints you no longer need.

!!! warning "Toggle Active leaves the sketch under-constrained"
    **Cause:** Disabling a constraint that was removing a DoF leaves that DoF free.
    The sketch reverts to under-constrained (white elements).  
    **Fix:** This is expected behaviour — Toggle Active is for exploration. Re-enable
    the constraint to restore full constraint.

---

## Python API

### Toggle Driving

```python
import FreeCAD as App

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Toggle constraint index 2 to reference mode
sketch.setDriving(2, False)

# Toggle back to driving
sketch.setDriving(2, True)

doc.recompute()
```

### Toggle Active

```python
# Toggle constraint index 1 active/inactive
sketch.setActive(1, False)  # disable
sketch.setActive(1, True)   # re-enable
doc.recompute()
```

---

## See also

- [Dimension](dimension.md) — set driving/reference at creation time
- [Constraint Distance](constraint-distance.md) — dimensional constraint to toggle
- [Constraint Radius/Diameter](constraint-radius-diameter.md) — dimensional constraint to toggle
