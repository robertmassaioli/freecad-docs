# Simulation

> **In one sentence:** Drive one or more joints through a defined range of
> motion frame-by-frame and watch the mechanism animate — without needing an
> external physics engine.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Simulation  
**Shortcut:** none

---

## Intuition

The constraint solver in the Assembly workbench is a *static* solver: given
a fixed set of joint values, it finds the one position where all constraints
are satisfied. Simulation extends this into *kinematic animation* by
repeatedly re-solving the assembly while incrementally changing one or more
driving joint values (the *drivers*).

The result is a smooth frame-by-frame animation of the mechanism moving
through its range of motion — a four-bar linkage cycling, a piston engine
reciprocating, a robotic arm sweeping through a workspace. No external
physics engine or scripting is required.

**What simulation does not do:**

- It does not compute forces, stresses, or dynamics (mass × acceleration).
  It is purely kinematic — it shows where parts are, not how hard they push.
- It does not detect collisions or physically prevent parts from passing
  through each other. Interpenetration is possible if the mechanism is
  geometrically degenerate.

For force and stress analysis, use FreeCAD's FEM workbench. For collision
detection, use a dedicated CAD or simulation tool.

---

## Setting up a simulation

### Prerequisites

1. The assembly must be solved (Solve Assembly run successfully).
2. The mechanism must have exactly the right DOF for simulation: one
   remaining free DOF per driver joint.  
   A mechanism with 0 free DOF is fully constrained — nothing can move.  
   A mechanism with 2+ free DOF needs two driver joints to be fully defined.

### Creating a simulation

1. Choose **Assembly → Simulation**. The Simulation task panel opens.
2. The panel shows a **Joints** list of all joints in the assembly.
3. Click **Add Driver** and select a joint to drive.
4. Set the **Start**, **End**, and **Steps** (number of frames) for that
   driver's range:

   | Field | Description |
   |-------|-------------|
   | Start | Initial value of the driven DOF (degrees for rotation, mm for translation) |
   | End | Final value of the driven DOF |
   | Steps | Number of frames to compute (higher = smoother animation) |

5. Repeat **Add Driver** for additional drivers if the mechanism requires
   more than one.

### Playback controls

- **Play** — run the animation from start to end.
- **Stop** — halt playback.
- **Step** slider — drag to jump to any frame manually.
- **Loop** — toggle continuous looping.
- **Speed** — adjust playback speed (frame rate multiplier).

---

## Driver joint types

| Joint type | Driven value | Unit |
|------------|--------------|------|
| Revolute | Rotation angle | degrees |
| Slider | Translation distance | mm |
| Cylindrical | Rotation or translation | degrees / mm |
| Any joint with a Limit parameter | The limit value | joint-dependent |

Coupled joints (Gears, Belt, Screw, Rack and Pinion) respond automatically
to their driving joint — you drive the input joint and the coupled output
follows.

---

## Exporting the animation

The Simulation task panel does not directly export a video file. To create
a video from a FreeCAD assembly simulation:

1. Play the simulation and use your operating system's screen recording
   tool to capture the 3-D view.
2. Or export each frame as an image using a macro that steps through frames
   and saves screenshots, then stitch with a video editor.

---

## Multiple simulations

A document can contain multiple simulation definitions (e.g. one for a full
cycle, one for a partial range). Each is stored as a separate
`Assembly::SimulationObject` in the model tree. Double-click a simulation
object to re-enter its task panel.

---

## Common mistakes and pitfalls

!!! warning "Mechanism must be exactly constrained minus driver DOF"
    If the mechanism has 2 residual free DOF and you add only 1 driver, the
    remaining free DOF makes the simulation indeterminate — parts can be at
    any of multiple positions for each driver value. Add a second driver or
    add another joint to reduce free DOF to 1.

!!! warning "Driving the wrong joint"
    The driver joint must be one that *directly* drives the mechanism's
    free DOF. If you drive a joint that is part of a fully-constrained
    sub-chain, the solver will report over-constrainment at every frame.

!!! warning "Coupled joints need their base joints driven"
    A Gears coupled joint links two Revolute joints. To drive the gear
    mechanism, add a driver on one of the Revolute joints (the input gear),
    not on the Gears coupled joint itself.

!!! warning "Large step counts slow down playback"
    Each step requires a full solver run. Very large step counts (>500) can
    make playback slow. Use 50–100 steps for smooth animation in most
    mechanisms; increase only if fine resolution is needed.

---

## Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

# Open the Simulation task panel via GUI command
Gui.doCommand('import CommandCreateSimulation')
Gui.doCommand('CommandCreateSimulation.CommandCreateSimulation().Activated()')

# Simulation objects are stored as Assembly::SimulationObject.
# Programmatic frame stepping for headless export:
doc = App.ActiveDocument
sim = doc.getObject("Simulation")  # or its actual name

# Access simulation properties
# (exact API depends on FreeCAD version; use the task panel for setup)
```

---

## See also

- [Solve Assembly](solve.md) — prerequisites for running a simulation
- [Joints — Kinematic](joints-kinematic.md) — the driver joint types
- [Joints — Coupled Motion](joints-coupled.md) — coupled joints that follow drivers automatically
- [Exploded View](exploded-view.md) — static step-by-step animation (alternative to simulation)
