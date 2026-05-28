# Robot Tools

Complete reference for all Robot workbench tools.

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0. It is preserved for
    existing users but is no longer actively maintained.

---

## Robot Setup

| Tool | Description |
|------|-------------|
| [Robot Tool](robot-tool.md) | Attach a shape as the robot's end-effector at the flange |
| [Place Robot](place-robot.md) | Position and orient the robot in the scene |
| [Set Home Position](set-home-position.md) | Store the current joint angles as the home configuration |
| [Move to Home](move-to-home.md) | Return the robot to its stored home configuration |

---

## Trajectory Creation

| Tool | Description |
|------|-------------|
| [Trajectory](trajectory.md) | Create a new empty trajectory object |
| [Insert in Trajectory (Tool Position)](insert-trajectory-tool-position.md) | Append the current TCP as a waypoint |
| [Insert in Trajectory (Preselect)](insert-trajectory-preselect.md) | Add the cursor-highlighted position as a waypoint |
| [Set Default Orientation](set-default-orientation.md) | Set the default tool orientation for new waypoints |
| [Set Default Values](set-default-values.md) | Set default velocity and acceleration for new waypoints |
| [Edge to Trajectory](edge-to-trajectory.md) | Auto-generate waypoints from selected geometry edges |

---

## Trajectory Management

| Tool | Description |
|------|-------------|
| [Dress-Up Trajectory](dress-up-trajectory.md) | Override velocity or orientation at specific waypoints |
| [Trajectory Compound](trajectory-compound.md) | Concatenate multiple trajectories into one |

---

## Simulation and Export

| Tool | Description |
|------|-------------|
| [Simulate Trajectory](simulate-trajectory.md) | Animate the robot through the trajectory |
| [Kuka Compact Subroutine](kuka-compact-subroutine.md) | Export as minimal KRL |
| [Kuka Full Subroutine](kuka-full-subroutine.md) | Export as complete KRL with declarations |
