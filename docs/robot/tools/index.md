# Robot Tools

## Robot Setup

| Tool | Description |
|------|-------------|
| [Tool](tools.md#tool) | Attach a shape as the robot's tool at the flange |
| [Place Robot](tools.md#place-robot) | Position and orient the robot in the scene |
| [Set Home Position](tools.md#set-home-position) | Store the current joint angles as the home configuration |
| [Move to Home](tools.md#move-to-home) | Return the robot to its stored home configuration |

## Trajectory

| Tool | Description |
|------|-------------|
| [Trajectory](tools.md#trajectory) | Create a new empty trajectory |
| [Insert in Trajectory (tool pos)](tools.md#insert-in-trajectory) | Add the current TCP as a waypoint |
| [Insert in Trajectory (preselect)](tools.md#insert-in-trajectory-preselect) | Add the preselected position as a waypoint |
| [Set Default Orientation](tools.md#set-default-orientation) | Set the default tool orientation for new waypoints |
| [Set Default Values](tools.md#set-default-values) | Set default velocity and acceleration for new waypoints |
| [Edge to Trajectory](tools.md#edge-to-trajectory) | Generate waypoints from selected edges |
| [Dress-Up Trajectory](tools.md#dress-up-trajectory) | Override speed or orientation across a trajectory |
| [Trajectory Compound](tools.md#trajectory-compound) | Concatenate multiple trajectories into one |

## Simulation and Export

| Tool | Description |
|------|-------------|
| [Simulate Trajectory](tools.md#simulate-trajectory) | Run a kinematic simulation of the trajectory |
| [Kuka Compact Subroutine](tools.md#kuka-compact-subroutine) | Export trajectory as compact KRL |
| [Kuka Full Subroutine](tools.md#kuka-full-subroutine) | Export trajectory as full KRL with declarations |
