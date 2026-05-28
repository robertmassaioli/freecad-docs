# Kuka Full Subroutine

> **In one sentence:** Export a trajectory as a complete KRL programme with
> variable declarations, tool and base frame setup, and one motion instruction
> per waypoint.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Export Trajectory → Kuka Full Subroutine  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Writes the selected trajectory to a KRL `.src` file including full programme
structure: `DEF`/`END` block, `E6POS` and `E6AXIS` variable declarations for
all waypoints, `$TOOL` and `$BASE` setup, and one motion instruction per
waypoint. This output can be loaded directly onto a Kuka controller.

---

## Intuition

Where Compact KRL is a quick sketch, Full KRL is the complete programme: it
declares every variable, sets up the tool and base frames, and provides
everything the controller needs to run the programme independently. Use this
for actual robot deployment.

---

## When to use it

- You are generating a programme for deployment on a real Kuka controller.
- Your controller requires full variable declarations.
- You need `$TOOL` and `$BASE` frame setup in the programme.

---

## Step-by-step

1. Verify the trajectory with [Simulate Trajectory](simulate-trajectory.md).
2. Verify that `robot.Tool` and base frame match the controller configuration.
3. Select the `Robot::TrajectoryObject`.
4. Choose **Robot → Export Trajectory → Kuka Full Subroutine**.
5. Save the `.src` file.

---

## Output format example

```krl
DEF Trajectory()
  ; Tool and base declarations
  $TOOL = TOOL_DATA[1]
  $BASE = BASE_DATA[1]

  ; Point data
  DECL E6POS Pt1 = {X 100, Y 0, Z 500, A 0, B 0, C 0, E1 0, E2 0, E3 0, E4 0, E5 0, E6 0}

  ; Motion instructions
  LIN Pt1 Vel=0.5 m/s CPDAT1

END
```

---

## Common mistakes and pitfalls

!!! warning "Joint values look correct but TCP positions are wrong"
    **Cause:** The `robot.Tool` or base frame number does not match the
    controller's configuration.  
    **Fix:** Verify that the tool number and base number in FreeCAD match the
    corresponding `TOOL_DATA` and `BASE_DATA` entries on the controller.

---

## See also

- [Kuka Compact Subroutine](kuka-compact-subroutine.md) — minimal KRL without declarations
- [Simulate Trajectory](simulate-trajectory.md) — verify before exporting
- [Robot Workbench](../index.md) — workbench overview
