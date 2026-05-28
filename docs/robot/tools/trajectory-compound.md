# Trajectory Compound

> **In one sentence:** Concatenate two or more trajectory objects into a single
> compound trajectory executed in order.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Trajectory Compound  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Combines selected `Robot::TrajectoryObject` instances into a
`Robot::TrajectoryCompoundObject`. The robot executes each sub-trajectory in
sequence, moving from the end of one to the start of the next.

---

## When to use it

- You have modular trajectory segments (approach, process, retract) and want
  to assemble them into a complete program.
- You want to reuse sub-trajectories across different compound programs.

---

## Step-by-step

1. Select two or more `Robot::TrajectoryObject` objects.
2. Choose **Robot → Trajectory Compound**.
3. A `Robot::TrajectoryCompoundObject` is created linking all selected
   trajectories in order.

---

## See also

- [Trajectory](trajectory.md) — create the sub-trajectory objects
- [Simulate Trajectory](simulate-trajectory.md) — simulate the compound
- [Robot Workbench](../index.md) — workbench overview
