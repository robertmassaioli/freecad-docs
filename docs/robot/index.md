# Robot Workbench

> **In one sentence:** The Robot workbench simulates 6-axis industrial robot
> motion — place a robot model in a scene, build a trajectory of waypoints,
> run a forward/inverse kinematics simulation, and export the result as a
> Kuka KRL programme.

---

## What is the Robot workbench for?

The Robot workbench models the motion of an **industrial 6-axis serial robot**
(like a Kuka, FANUC, or ABB arm). It is a path-planning and simulation tool,
not a motion-control interface — it does not connect to real robots.

**Common uses:**

- Verifying that a robot can reach all points on a welding or milling path
  without hitting joint limits or colliding with the fixture.
- Generating a KRL (Kuka Robot Language) programme skeleton from a trajectory
  defined in FreeCAD.
- Teaching yourself how industrial robot kinematics work by interactively
  setting joint angles and observing the TCP.

---

## Core concepts

### Robot model

A robot is represented by a `Robot::RobotObject` in the document. It stores:

- A **VRML file** for the visual mesh of each link.
- A **kinematic file** (`.csv` format) defining the Denavit–Hartenberg (DH)
  parameters for each joint.
- Six **joint angle properties** (`Axis1`–`Axis6`), in degrees.
- A **Tool placement** — the transformation from the flange to the tool centre
  point (TCP).
- A **Tcp property** — the current TCP placement in world coordinates.
- A **Home** property — the stored home joint angles.

The default robot (if no kinematic file is supplied) is a **Puma 560**.

### Forward and inverse kinematics

The `Robot6Axis` Python class performs real-time kinematics:

- **Forward kinematics (FK):** Set `Axis1`–`Axis6` → `Tcp` updates.
- **Inverse kinematics (IK):** Set `Tcp` → `Axis1`–`Axis6` update.

### Waypoints and trajectories

| Concept | Description |
|---------|-------------|
| **Waypoint** | A target TCP placement with name, motion type, velocity, and tool/base frame references |
| **Motion type: LIN** | Linear Cartesian interpolation to the target |
| **Motion type: PTP** | Point-to-point (joint-interpolated) motion |
| **Trajectory** | An ordered list of waypoints |
| **Trajectory Compound** | Multiple trajectories concatenated into one execution sequence |
| **Trajectory Dress-up** | Overrides applied on top of an existing trajectory (speed, orientation) |

### Kuka KRL export

The trajectory can be exported as:
- **Compact KRL** — a condensed subroutine, one line per waypoint.
- **Full KRL** — a complete subroutine with declarations, variable assignments,
  and tool/base frame setup.

---

## Workflow overview

```
Insert robot model
        │
        ▼
Place robot (set location in scene)
        │
        ▼
Set home position
        │
        ▼
Create empty trajectory
        │
        ├── Insert waypoints manually (set axes → insert TCP)
        │   or
        └── Generate from edges (Edge to Trajectory)
                │
                ▼
        Apply dress-up (speed, orientation overrides)
                │
                ▼
        Simulate trajectory
                │
                ▼
        Export to Kuka KRL
```

---

## See also

- [Robot Tools](tools/index.md) — all tools
