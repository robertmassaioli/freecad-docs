# Place Robot

> **In one sentence:** Position and orient a robot object in the 3-D scene by
> setting its base placement interactively.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Place Robot  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Opens a task panel to set the robot's world-frame position and orientation.
The robot's `Placement` property is updated so its base frame sits at the
specified location.

---

## Intuition

Before adding trajectories, the robot must be placed in the work cell at the
correct location — just as a real robot is mounted at a fixed position on the
factory floor. Place Robot sets that base position.

---

## When to use it

- When setting up a new robot simulation, before creating any trajectories.
- When repositioning a robot for a different work cell layout.

---

## Step-by-step

1. Select the `Robot::RobotObject`.
2. Choose **Robot → Place Robot**.
3. In the task panel, set the position (X, Y, Z in mm) and orientation (Euler
   angles or quaternion).
4. Click **OK**.

---

## Python API

```python
import FreeCAD as App
from FreeCAD import Vector, Rotation, Placement

robot = App.ActiveDocument.getObject("Robot")
robot.Placement = Placement(
    Vector(500, 0, 0),      # base position in mm
    Rotation(0, 0, 0, 1)    # identity rotation
)
App.ActiveDocument.recompute()
```

---

## See also

- [Set Home Position](set-home-position.md) — store the current joint configuration as home
- [Robot Workbench](../index.md) — workbench overview
