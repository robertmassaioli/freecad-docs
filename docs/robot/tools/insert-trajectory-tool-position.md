# Insert in Trajectory (Tool Position)

> **In one sentence:** Append the robot's current TCP placement as a new
> waypoint to the selected trajectory.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Insert in Trajectory  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Reads the current TCP (Tool Centre Point) from the selected robot and appends
it as a new waypoint to the selected trajectory. The waypoint's motion type
(LIN or PTP), velocity, and frame assignments use the current default values.

---

## Intuition

You manually jog the robot to the position you want it to visit, then press
this button to record that position in the trajectory. Repeat for each target
point.

---

## When to use it

- Creating a trajectory interactively by positioning the robot at each
  target and recording the pose.

---

## Step-by-step

1. Select one `Robot::RobotObject` and one `Robot::TrajectoryObject`.
2. Use the Robot Control panel to set the joint angles to the desired pose.
3. Confirm the TCP is at the intended position.
4. Choose **Robot → Insert in Trajectory**.
5. A new waypoint is added at the end of the trajectory.

---

## See also

- [Insert in Trajectory (Preselect)](insert-trajectory-preselect.md) — add a clicked position
- [Set Default Values](set-default-values.md) — set default velocity for new waypoints
- [Trajectory](trajectory.md) — create the trajectory object first
- [Robot Workbench](../index.md) — workbench overview
