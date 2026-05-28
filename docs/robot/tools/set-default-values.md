# Set Default Values

> **In one sentence:** Set the default velocity, acceleration, and continuity
> applied to all subsequently inserted trajectory waypoints.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Set Default Values  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Opens a dialog to configure the default motion parameters for new waypoints:
velocity, acceleration, and continuity (stop vs fly-through).

---

## When to use it

- Before recording waypoints, to set the motion speed for the path segment.
- When switching between slow precision moves and fast transit moves.

---

## Step-by-step

1. Choose **Robot → Set Default Values**.
2. In the dialog, set:
   - **Velocity** — percentage of max speed or mm/s for LIN moves.
   - **Acceleration** — percentage of max acceleration.
   - **Continuity** — whether the robot stops exactly at each waypoint
     (stop) or flies through at speed (continuous).
3. Click **OK**.
4. All subsequently inserted waypoints inherit these defaults.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Velocity | Percentage of maximum speed (0–100%) or absolute speed in mm/s for LIN moves |
| Acceleration | Percentage of maximum acceleration |
| Continuity | Stop (exact point stop) or fly-through (continuous blending) |

---

## See also

- [Set Default Orientation](set-default-orientation.md) — set default tool orientation
- [Dress-Up Trajectory](dress-up-trajectory.md) — override values on an existing trajectory
- [Robot Workbench](../index.md) — workbench overview
