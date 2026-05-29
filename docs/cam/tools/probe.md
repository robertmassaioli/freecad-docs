# Probe

> **In one sentence:** Generate a touch-off probing routine that measures
> workpiece position and updates the work coordinate system.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Probe  
**Command:** `CAM_Probe`  
**Shortcut:** none

Generates a probing cycle that uses a touch probe to measure the workpiece
position and update the Work Coordinate System (WCS). The resulting G-code
uses G38.2 (or similar) probing cycles supported by the machine controller.

---

## Intuition

When you load a workpiece onto the machine, its exact position is never
perfectly known. A probing routine moves the probe to known reference surfaces,
measures the actual position, and stores the offset — so subsequent operations
run in the correct coordinate system even if the part was not placed exactly.

---

## Requirements

- A probe tool bit (see [Tool Bit Library](tool-bit-library.md)) must be selected.
- The machine controller must support probing cycles (G38.2 or equivalent).
- The post-processor must output the correct probing syntax for your controller.

---

## When to use it

- Automatically locating a workpiece datum before running a programme.
- Checking workpiece dimensions in-cycle (in-process gauging).
- Compensating for fixture position variation between setups.

---

## Common mistakes and pitfalls

!!! warning "Machine controller does not support probing"
    Not all CNC controllers support G38.2 probing cycles. Check your controller
    documentation before adding a Probe operation.

!!! warning "Wrong probe tool selected"
    If the Tool Controller does not reference a probe tool bit, the generated
    G-code will reference the wrong tool number. Create a dedicated probe tool
    and tool controller.

---

## See also

- [Custom](custom.md) — insert arbitrary G-code
- [Stop](stop.md) — insert a programme pause
- [Tool Controller](tool-controller.md) — configure tool feeds, speeds, and tool number
- [CAM Workbench](../index.md) — workbench overview
