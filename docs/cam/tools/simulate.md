# Simulate

> **In one sentence:** Animate the toolpath and show the material removed
> from the stock as the virtual tool moves through each operation.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Simulators  
**Commands:** `CAM_SimulatorGL` (GL Simulator), `CAM_Simulator` (legacy)  
**Shortcut:** none

Two simulators are available: the **GL Simulator** (default, OpenGL-based,
real-time) and the **legacy Simulator** (Python-based, slower). Both animate
the toolpath and show the resulting machined shape.

---

## Intuition

Simulation lets you visually verify that the toolpath cuts the right geometry
before sending G-code to the machine. It catches crashes (tool hitting the
fixture), gouges (cutting into unintended geometry), and missed material —
all without risking a broken tool or damaged workpiece.

Always simulate before post-processing.

---

## When to use it

- Before post-processing to verify the complete toolpath.
- After adding dress-ups to check their effect.
- When debugging an unexpected operation result.

---

## GL Simulator (default)

The GL Simulator renders material removal in real time using OpenGL. The stock
starts as a solid block and the tool is animated, removing material as it moves.

| Control | Action |
|---------|--------|
| Play / Pause | Start/stop animation |
| Speed slider | Playback speed multiplier |
| Step | Advance one G-code line |
| Show tool | Toggle tool mesh visibility |

## Legacy Simulator

The older Python-based simulator is slower but works without hardware OpenGL.
It shows the resulting mesh after all operations are applied rather than
animating in real time.

---

## Common mistakes and pitfalls

!!! warning "Simulation looks correct but machine crashes"
    Simulation uses the CAM model geometry. If your actual setup (fixture,
    clamps, table surface) is not modelled in FreeCAD, simulation cannot
    detect collisions with those items. Always leave adequate clearance.

---

## See also

- [Inspect Path](inspect-path.md) — view raw G-code for detailed debugging
- [Post Process](post-process.md) — generate G-code after simulation passes
- [Toggle Active](toggle-active.md) — disable operations for partial simulation
- [CAM Workbench](../index.md) — workbench overview
