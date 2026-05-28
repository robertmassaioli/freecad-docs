# Move to Home

> **In one sentence:** Return the robot to its stored home joint configuration
> by restoring `Axis1`–`Axis6` from the `Home` property.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Move to Home  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Reads the `Home` property (set by [Set Home Position](set-home-position.md))
and sets `Axis1`–`Axis6` to those values. The TCP (tool centre point) updates
via forward kinematics.

---

## When to use it

- Before a simulation run, to reset the robot to its start pose.
- After manually moving joints for inspection, to return to the reference
  configuration.

---

## Step-by-step

1. Ensure the robot has a stored home position (set via
   [Set Home Position](set-home-position.md)).
2. Choose **Robot → Move to Home**.
3. The robot's joints snap to the stored home values.

---

## Python API

```python
import FreeCAD as App

robot = App.ActiveDocument.getObject("Robot")
home = robot.Home   # [a1, a2, a3, a4, a5, a6]
robot.Axis1 = home[0]
robot.Axis2 = home[1]
robot.Axis3 = home[2]
robot.Axis4 = home[3]
robot.Axis5 = home[4]
robot.Axis6 = home[5]
App.ActiveDocument.recompute()
```

---

## See also

- [Set Home Position](set-home-position.md) — store the home configuration
- [Robot Workbench](../index.md) — workbench overview
