# Kuka Compact Subroutine

> **In one sentence:** Export a trajectory as a minimal KRL subroutine with
> one motion instruction per waypoint.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Export Trajectory → Kuka Compact Subroutine  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Writes the selected trajectory to a KRL (Kuka Robot Language) `.src` file.
Each waypoint becomes one motion instruction (`PTP` or `LIN`) with its
position, velocity, and continuity. Suitable for controllers that accept
compact KRL without variable declarations.

---

## Intuition

Compact KRL is the minimal form: each line says "move here, at this speed,
using this motion type" — no preambles, no variable declarations, no `$TOOL`
or `$BASE` setup. Use this for simple trajectories or when your post-processor
handles the rest.

---

## When to use it

- You need a quick export to test on a Kuka controller.
- Your controller accepts compact KRL without full variable declarations.
- You are comparing the motion output to a manual programme.

---

## When NOT to use it

- **For production programmes** — use [Kuka Full Subroutine](kuka-full-subroutine.md)
  which includes proper variable declarations and `$TOOL`/`$BASE` setup.

---

## Step-by-step

1. Verify the trajectory with [Simulate Trajectory](simulate-trajectory.md).
2. Select the `Robot::TrajectoryObject`.
3. Choose **Robot → Export Trajectory → Kuka Compact Subroutine**.
4. A save dialog appears. Choose a file name with `.src` extension.
5. Click **Save**.

---

## Output format example

```krl
DEF Trajectory()
  PTP {A1 0, A2 -90, A3 90, A4 0, A5 0, A6 0} Vel=100% PDAT1
  LIN {X 100, Y 0, Z 500, A 0, B 0, C 0} Vel=2 m/s CPDAT1
END
```

---

## See also

- [Kuka Full Subroutine](kuka-full-subroutine.md) — complete KRL with variable declarations
- [Simulate Trajectory](simulate-trajectory.md) — verify before exporting
- [Robot Workbench](../index.md) — workbench overview
