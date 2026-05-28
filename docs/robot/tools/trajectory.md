# Trajectory

> **In one sentence:** Create a new, empty trajectory object that will hold
> an ordered sequence of robot waypoints.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Trajectory  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Creates a `Robot::TrajectoryObject` in the document. The trajectory starts
empty; waypoints are added with [Insert in Trajectory](insert-trajectory-tool-position.md),
[Insert in Trajectory (preselect)](insert-trajectory-preselect.md), or
[Edge to Trajectory](edge-to-trajectory.md).

---

## Intuition

A trajectory is a script for the robot — an ordered list of poses the robot
must visit, with motion type (PTP or LIN), velocity, and continuity settings
at each pose. Trajectory creates the empty script; the other tools fill it in.

---

## When to use it

- Before adding any waypoints — you must have a trajectory object first.
- When you need multiple separate motion programs (create one trajectory per
  program).

---

## Step-by-step

1. Choose **Robot → Trajectory**.
2. A `Robot::TrajectoryObject` named "Trajectory" is created in the document.
3. Use Insert in Trajectory or Edge to Trajectory to add waypoints.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
traj = doc.addObject("Robot::TrajectoryObject", "Trajectory")
print(traj.Trajectory.Waypoints)  # []  — starts empty
doc.recompute()
```

---

## See also

- [Insert in Trajectory (tool position)](insert-trajectory-tool-position.md) — add the current TCP as a waypoint
- [Edge to Trajectory](edge-to-trajectory.md) — auto-generate waypoints from geometry
- [Simulate Trajectory](simulate-trajectory.md) — animate the trajectory
- [Robot Workbench](../index.md) — workbench overview
