# Insert in Trajectory (Preselect Position)

> **In one sentence:** Add the position currently highlighted under the cursor
> in the 3-D view as a new trajectory waypoint, without moving the robot first.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Insert in Trajectory  
**Shortcut:** W (when robot and trajectory are selected)

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Inserts the current mouse-preselected position (the point on geometry
highlighted as the cursor hovers over it) directly into the trajectory.
The robot does not need to be jogged to the position first.

---

## Intuition

Instead of manually setting joint angles to reach a point, you just hover
over the target point on a surface in the 3-D view and press W. The point
under your cursor is recorded as a waypoint immediately.

---

## When to use it

- Creating trajectories by clicking directly on target surfaces without
  manually jogging the robot.
- Quickly recording a series of points on a part surface for a spray, weld,
  or inspection path.

---

## Step-by-step

1. Select one `Robot::RobotObject` and one `Robot::TrajectoryObject`.
2. Hover the cursor over the target point on a geometry surface.
3. Press **W** (or choose **Robot → Insert in Trajectory** while hovering).
4. The preselected position is recorded as a waypoint.

---

## See also

- [Insert in Trajectory (Tool Position)](insert-trajectory-tool-position.md) — record the robot's current TCP
- [Edge to Trajectory](edge-to-trajectory.md) — auto-generate from edges
- [Robot Workbench](../index.md) — workbench overview
