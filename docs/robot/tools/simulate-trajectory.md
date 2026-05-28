# Simulate Trajectory

> **In one sentence:** Animate the robot moving through every waypoint in a
> trajectory, computing joint angles via inverse kinematics at each step.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Simulate Trajectory  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Opens the Trajectory Simulate task panel. Playback controls let you play,
pause, step, and scrub through the trajectory. The robot's visual model
updates in real time. If any waypoint is kinematically unreachable, simulation
halts at that waypoint with an error.

---

## Intuition

Simulate Trajectory is the "Play" button for robot programming. It checks
whether the robot can actually reach all the waypoints you defined, visualises
the motion, and catches kinematic problems before you send code to a real robot.

---

## When to use it

- After creating a trajectory, to verify reach, clearance, and joint limits.
- Before generating KRL export code, to confirm the program is correct.

---

## Step-by-step

1. Select a `Robot::RobotObject` and a `Robot::TrajectoryObject` (or
   `Robot::TrajectoryCompoundObject`).
2. Choose **Robot → Simulate Trajectory**.
3. The Simulate task panel opens.
4. Click **Play** to run the simulation.
5. Use **Step forward/backward** to inspect individual waypoints.
6. If an error occurs, the waypoint index is shown — check that waypoint's
   position in the workspace.

---

## Simulation controls

| Control | Action |
|---------|--------|
| Play | Run animation from current position to end |
| Stop | Pause playback |
| Step forward | Advance one waypoint |
| Step backward | Go back one waypoint |
| Speed slider | Adjust playback speed |

---

## Common mistakes and pitfalls

!!! warning "Simulation stops with kinematic error"
    **Cause:** A waypoint is outside the robot's reachable workspace or
    exceeds joint limits.  
    **Fix:** Reposition the waypoint closer to the robot, or adjust the
    robot's base placement. Check DH parameters and joint limits.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Robot_Simulate', 0)")
```

---

## See also

- [Trajectory](trajectory.md) — the trajectory to simulate
- [Kuka Compact Subroutine](kuka-compact-subroutine.md) — export after simulation verification
- [Robot Workbench](../index.md) — workbench overview
