# Robot Tools

---

## Tool {#tool}

**Menu:** Robot → Insert Robot → Tool  
**Command:** `Robot_AddToolShape`

Attaches a `Part::Feature` or VRML shape as the robot's **tool** — the
physical end-effector mounted on the robot flange (e.g. a weld gun, gripper,
or milling spindle). The shape is displayed attached to the robot's flange
during simulation.

### Selection

Select **one** `Robot::RobotObject` and **one** `Part::Feature` (or VRML
object). FreeCAD sets `robot.ToolShape = shape`.

---

## Place Robot {#place-robot}

**Menu:** Robot → Place Robot  
**Command:** `Robot_ConstraintAxle` (internally sets robot placement)

Opens a task panel to position and orient the robot in the 3-D scene. The
robot's `Placement` property is set so its base is at the specified location
and orientation.

---

## Set Home Position {#set-home-position}

**Menu:** Robot → Set Home Position  
**Command:** `Robot_SetHomePos`

Stores the current `Axis1`–`Axis6` values as the robot's **home
configuration**. The home is saved to the `Home` property as a list of six
floats.

The home position is the configuration the robot returns to between jobs. For
simulation, it is also the starting configuration if you run the simulator
after clicking **Move to Home**.

---

## Move to Home {#move-to-home}

**Menu:** Robot → Move to Home  
**Command:** `Robot_RestoreHomePos`

Sets `Axis1`–`Axis6` from the stored `Home` property, moving the robot back
to its home configuration. The Tcp updates via forward kinematics.

---

## Trajectory {#trajectory}

**Menu:** Robot → Trajectory  
**Command:** `Robot_CreateTrajectory`

Creates a new, empty `Robot::TrajectoryObject` in the document. An empty
trajectory has no waypoints; add them with the Insert in Trajectory or Edge
to Trajectory tools.

### Python API

```python
import Robot
import FreeCAD as App

doc = App.ActiveDocument
trac = doc.addObject("Robot::TrajectoryObject", "Trajectory")
# Trajectory starts empty:
print(trac.Trajectory.Waypoints)  # []
```

---

## Insert in Trajectory (tool position) {#insert-in-trajectory}

**Menu:** Robot → Insert in Trajectory  
**Command:** `Robot_InsertWaypoint`

Reads the current **TCP placement** from the selected robot and appends it
as a new waypoint to the selected trajectory. The waypoint type (LIN or PTP),
velocity, and default tool/base frames come from the current defaults.

### Selection

Select one `Robot::RobotObject` and one `Robot::TrajectoryObject`.

### Workflow

1. Use the **Robot Control** task panel (activated when a robot is selected)
   to set the joint angles.
2. Confirm the TCP is where you want the waypoint.
3. Run **Insert in Trajectory**.
4. Repeat for each target point.

---

## Insert in Trajectory (preselect position) {#insert-in-trajectory-preselect}

**Menu:** Robot → Insert in Trajectory  
**Command:** `Robot_InsertWaypointPreselect`  
**Shortcut:** W (when robot and trajectory are selected)

Inserts the current **preselected position** (the point highlighted under the
cursor in the 3-D view) as a waypoint in the trajectory, rather than the
robot's current TCP. This allows you to click a point on geometry and
immediately record it as a waypoint without moving the robot first.

---

## Set Default Orientation {#set-default-orientation}

**Menu:** Robot → Set Default Orientation  
**Command:** `Robot_SetDefaultOrientation`

Sets the **default tool orientation** used when inserting new waypoints. New
waypoints will use this orientation unless overridden individually. This is
useful when most waypoints in a trajectory share the same approach angle (e.g.
always perpendicular to a surface).

---

## Set Default Values {#set-default-values}

**Menu:** Robot → Set Default Values  
**Command:** `Robot_SetDefaultValues`

Sets the **default velocity** and **default acceleration** applied to new
waypoints. Opens a dialog to enter:

| Value | Description |
|-------|-------------|
| **Velocity** | Percentage of maximum speed (0–100 %) or mm/s for LIN moves |
| **Acceleration** | Percentage of maximum acceleration |
| **Continuity** | Whether each waypoint is reached exactly (stop) or flown through |

---

## Edge to Trajectory {#edge-to-trajectory}

**Menu:** Robot → Edge to Trajectory  
**Command:** `Robot_Edge2Trac`

Generates a trajectory automatically from **selected edges** on a shape.
FreeCAD discretises each edge into a sequence of waypoints at a specified
point density. This is the primary tool for generating machining or welding
paths along a geometric feature.

### Task panel parameters

| Parameter | Description |
|-----------|-------------|
| **Point distance** | Distance between consecutive waypoints along each edge (mm). Smaller = more waypoints. |
| **Minimum point distance** | Skip points closer than this threshold (avoids dense clusters at edge junctions). |
| **Velocity** | Default velocity for generated waypoints. |
| **Base frame** | Reference frame for the trajectory. |
| **Tool frame** | Tool frame for the trajectory. |

### Step-by-step

1. Select the edges you want to follow (Ctrl+click).
2. Also select a `Robot::RobotObject` and a `Robot::TrajectoryObject`.
3. Go to **Robot → Edge to Trajectory**.
4. Set the point distance and velocity.
5. Click **OK**. FreeCAD creates `Robot::Edge2TracObject` and populates the
   trajectory with generated waypoints.

### Python API

```python
import Robot, FreeCAD as App

doc = App.ActiveDocument
robot = doc.getObject("Robot")
trac  = doc.getObject("Trajectory")

e2t = doc.addObject("Robot::Edge2TracObject", "EdgeTraj")
e2t.RobotObject  = robot
e2t.TrajectoryObject = trac
e2t.PointDistance = 1.0   # mm
doc.recompute()
```

---

## Dress-Up Trajectory {#dress-up-trajectory}

**Menu:** Robot → Dress-Up Trajectory  
**Command:** `Robot_TrajectoryDressUp`

Creates a `Robot::TrajectoryDressUpObject` that **overrides specific properties**
of an existing trajectory without modifying the original. Dress-ups can change:

- Velocity at selected waypoints.
- Tool orientation at selected waypoints.
- Continuity flags.

This allows the base trajectory to be regenerated (e.g. from edges) while
keeping manual overrides intact in the dress-up layer.

---

## Trajectory Compound {#trajectory-compound}

**Menu:** Robot → Trajectory Compound  
**Command:** `Robot_TrajectoryCompound`

Combines two or more `Robot::TrajectoryObject` objects into a single
`Robot::TrajectoryCompoundObject`. The compound executes the trajectories
in the order listed, with the robot moving between the end of one trajectory
and the start of the next.

### Selection

Select two or more `Robot::TrajectoryObject` objects.

---

## Simulate Trajectory {#simulate-trajectory}

**Menu:** Robot → Simulate Trajectory  
**Command:** `Robot_Simulate`

Opens the **Trajectory Simulate** task panel, which animates the robot moving
through each waypoint of the selected trajectory. The robot's joint angles and
TCP update in real time as the simulation plays.

### Simulation controls

| Control | Action |
|---------|--------|
| **Play** | Start animation |
| **Stop** | Pause |
| **Step forward** | Advance one waypoint |
| **Step back** | Go back one waypoint |
| **Speed slider** | Adjust playback speed |

The simulation uses the robot's kinematic model to compute joint angles at each
waypoint (inverse kinematics). If a waypoint is unreachable, the simulation
stops and reports an error.

---

## Kuka Compact Subroutine {#kuka-compact-subroutine}

**Menu:** Robot → Export Trajectory → Kuka Compact Subroutine  
**Command:** `Robot_ExportKukaCompact`

Exports the selected trajectory as a **compact KRL subroutine** — a minimal
Kuka Robot Language (KRL) file with one motion instruction per waypoint.

Output format example:
```krl
DEF Trajectory()
  PTP {A1 0, A2 -90, A3 90, A4 0, A5 0, A6 0} Vel=100% PDAT1
  LIN {X 100, Y 0, Z 500, A 0, B 0, C 0} Vel=2 m/s CPDAT1
  ...
END
```

---

## Kuka Full Subroutine {#kuka-full-subroutine}

**Menu:** Robot → Export Trajectory → Kuka Full Subroutine  
**Command:** `Robot_ExportKukaFull`

Exports the trajectory as a **full KRL subroutine** including:

- Variable declarations for all point data (`E6POS`, `E6AXIS`).
- Tool and base frame declarations.
- One motion instruction per waypoint.
- Proper KRL programme structure (`DEF` / `END` block with `$TOOL` and
  `$BASE` setup).

This output can be loaded directly into a Kuka controller (after verifying
joint limits and speeds on the actual robot).

---

## Python API

### Building a trajectory programmatically

```python
from Robot import Robot6Axis, Waypoint, Trajectory
from FreeCAD import Placement, Vector, Rotation
import FreeCAD as App

doc = App.newDocument()

# Create the kinematic robot object
rob = Robot6Axis()          # Default Puma 560 kinematics

# Build waypoints
waypoints = []
for i in range(6):
    pos = Placement(
        Vector(200, 0, 300 + i * 50),
        Rotation(Vector(0, 0, 1), 0)
    )
    wp = Waypoint(pos, "LIN", f"Pt{i}")
    wp.Velocity = 0.5       # 0.5 m/s
    waypoints.append(wp)

# Assemble trajectory
trac = Trajectory(waypoints)

# Put it in the document
robot_obj = doc.addObject("Robot::RobotObject", "Robot")
trac_obj  = doc.addObject("Robot::TrajectoryObject", "Traj")
trac_obj.Trajectory = trac
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Simulation stops with 'kinematic error'"
    **Cause:** The waypoint is outside the robot's reachable workspace, or
    requires a joint angle beyond the robot's limits.  
    **Fix:** Reposition the waypoint closer to the robot, or check the DH
    parameters and joint limits in the kinematic file.

!!! warning "Edge to Trajectory produces too many/few waypoints"
    **Cause:** The Point distance is set too small (dense) or too large (sparse)
    for the edge's length.  
    **Fix:** Adjust the Point distance in the Edge to Trajectory task panel.
    A good starting value is 1–5 % of the total path length.

!!! warning "KRL export produces joint values, but positions look wrong"
    **Cause:** The robot's Tool and Base frames are not set up to match the
    controller's configuration.  
    **Fix:** Verify that `robot.Tool` and the base frame match the
    corresponding tool number and base number on the real controller.
