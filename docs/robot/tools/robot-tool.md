# Robot Tool (Define Tool Shape)

> **In one sentence:** Attach a Part shape or VRML model as the robot's
> end-effector, displayed at the robot's flange during simulation.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Insert Robot → Tool  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0. It is preserved for
    existing users but is no longer maintained. For modern robot simulation,
    consider external tools.

Sets the `ToolShape` property of a `Robot::RobotObject` to a selected
`Part::Feature` or VRML shape. The shape is then displayed attached to the
robot flange joint in all views and during trajectory simulation.

---

## Intuition

A robot arm is just a chain of joints — it has no physical end-effector by
default. Robot Tool lets you attach a visual representation of the actual
tool (weld gun, gripper, spindle) so that trajectory simulations show what
the tool looks like and where it is in space.

---

## When to use it

- You want the simulation to show the physical tool shape at the robot's
  flange for visual verification of reach and clearance.
- You need to check TCP (Tool Centre Point) placement relative to the tool
  body geometry.

---

## Step-by-step

1. Select **one** `Robot::RobotObject` and **one** `Part::Feature` (or VRML
   object) in the document tree.
2. Choose **Robot → Insert Robot → Tool**.
3. The shape is set as the robot's tool. It appears at the flange in the
   3-D view.

---

## Parameters

| Input | Description |
|-------|-------------|
| Robot object | The `Robot::RobotObject` to attach the tool to |
| Tool shape | A `Part::Feature` or VRML object representing the end-effector |

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
robot = doc.getObject("Robot")
tool_shape = doc.getObject("GripperBody")

robot.ToolShape = tool_shape
doc.recompute()
```

---

## See also

- [Place Robot](place-robot.md) — position the robot in the scene
- [Robot Workbench](../index.md) — workbench overview
