# Set Home Position

> **In one sentence:** Store the robot's current joint angles as its home
> configuration for use as the simulation start pose.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Set Home Position  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Saves the current values of `Axis1`–`Axis6` to the robot's `Home` property
as a list of six floats. The home position is used as the starting configuration
when Move to Home is invoked.

---

## When to use it

- After positioning the robot at its preferred neutral pose, to save it as
  the home reference.
- Before a simulation run, to define the starting joint configuration.

---

## Step-by-step

1. Use the Robot Control panel to set the desired joint angles.
2. Confirm the robot is at the target home pose.
3. Choose **Robot → Set Home Position**.
4. The `Home` property is updated with the current `Axis1`–`Axis6` values.

---

## Python API

```python
import FreeCAD as App

robot = App.ActiveDocument.getObject("Robot")
# Set home from current axis values
robot.Home = [robot.Axis1, robot.Axis2, robot.Axis3,
              robot.Axis4, robot.Axis5, robot.Axis6]
App.ActiveDocument.recompute()
```

---

## See also

- [Move to Home](move-to-home.md) — return the robot to the stored home position
- [Place Robot](place-robot.md) — set the robot's world-frame position
- [Robot Workbench](../index.md) — workbench overview
