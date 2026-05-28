# Edge to Trajectory

> **In one sentence:** Automatically generate a series of trajectory waypoints
> by discretising selected geometry edges at a specified point density.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Edge to Trajectory  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Discretises each selected edge into evenly-spaced waypoints and populates
the selected trajectory with them. Creates a `Robot::Edge2TracObject` that
links the edge geometry to the trajectory — if the edges change, the waypoints
update automatically.

---

## Intuition

Instead of manually recording each waypoint along a weld seam or machining
path, you select the edges of the geometry and let FreeCAD compute the evenly-
spaced path points automatically. The denser the points, the more accurately
the robot will follow the edge's curvature.

---

## When to use it

- Generating welding, painting, or machining paths from existing CAD geometry.
- Creating inspection paths along part edges.
- Any task where the robot must follow a geometric feature rather than visit
  discrete manually chosen points.

---

## When NOT to use it

- **Complex 3-D curves requiring orientation control** — Edge to Trajectory
  computes positions but uses default orientation. Use
  [Dress-Up Trajectory](dress-up-trajectory.md) to override orientations at
  specific waypoints.

---

## Step-by-step

1. Select the edges to follow (Ctrl+click for multiple edges).
2. Also select a `Robot::RobotObject` and a `Robot::TrajectoryObject`.
3. Choose **Robot → Edge to Trajectory**.
4. In the task panel, set:
   - **Point distance** — spacing between waypoints in mm.
   - **Minimum point distance** — minimum distance to avoid clustering.
   - **Velocity** — default velocity for generated waypoints.
5. Click **OK**.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Point distance | Target spacing between consecutive waypoints along each edge (mm) |
| Min point distance | Skip points closer than this value to avoid dense clusters at junctions |
| Velocity | Default velocity for all generated waypoints |
| Base frame | Reference coordinate frame for the trajectory |
| Tool frame | Tool frame used for all generated waypoints |

---

## Common mistakes and pitfalls

!!! warning "Too many waypoints — point distance too small"
    **Cause:** Very small Point distance on long edges generates thousands of
    waypoints, slowing down simulation and KRL export.  
    **Fix:** Set Point distance to 1–5% of the total path length as a starting
    point.

!!! warning "Waypoints don't follow the edge smoothly"
    **Cause:** Point distance is too large relative to the edge's curvature —
    the robot will cut corners.  
    **Fix:** Reduce Point distance, especially in high-curvature regions.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
e2t = doc.addObject("Robot::Edge2TracObject", "EdgeTraj")
e2t.RobotObject = doc.getObject("Robot")
e2t.TrajectoryObject = doc.getObject("Trajectory")
e2t.PointDistance = 1.0   # mm
doc.recompute()
```

---

## See also

- [Insert in Trajectory (Tool Position)](insert-trajectory-tool-position.md) — manual waypoint recording
- [Dress-Up Trajectory](dress-up-trajectory.md) — override orientation at specific waypoints
- [Simulate Trajectory](simulate-trajectory.md) — visualise the result
- [Robot Workbench](../index.md) — workbench overview
