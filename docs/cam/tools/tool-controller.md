# Tool Controller

> **In one sentence:** A bridge between a tool bit definition and a Job
> operation — stores the feed rates, spindle speed, and tool number for
> a specific tool in a specific job.

---

## Overview

**Workbench:** CAM  
**Command:** `CAM_ToolController`

A Tool Controller (`CAM::ToolController`) connects a tool bit from the library
to a Job. While the tool bit file stores the tool's geometry, the Tool
Controller stores the cutting parameters for that tool in the context of the
current job: feed rates, spindle speed, and tool number.

---

## Intuition

The same end mill may be run at different speeds and feeds depending on the
material. The tool bit file is the geometry (diameter, flute count); the Tool
Controller is the recipe (RPM, feed rate) for a particular job. Multiple Tool
Controllers in the same job can reference the same tool bit with different
speeds and feeds.

---

## Tool Controller properties

| Property | Description |
|----------|-------------|
| Tool | Reference to the tool bit (`.fctb` file) |
| HorizFeed | Horizontal cutting feed rate (mm/min) |
| VertFeed | Vertical (plunge) feed rate (mm/min) |
| SpindleSpeed | Spindle speed (RPM) |
| SpindleDir | Clockwise (CW) or counter-clockwise (CCW) |

---

## Adding a Tool Controller

1. Open the [Tool Bit Library](tool-bit-library.md) or [Tool Bit Dock](tool-bit-dock.md).
2. Select a tool from the library.
3. Click **Add to Job** or drag the tool onto the Job.
4. A new Tool Controller appears in the Job's tool controller list.
5. Set the `HorizFeed`, `VertFeed`, `SpindleSpeed`, and `SpindleDir` properties.

## Assigning a Tool Controller to an operation

Each operation has a **Tool Controller** property. Click the dropdown in the
operation's properties and select from the job's tool controller list.

---

## Calculating feeds and speeds

FreeCAD does not auto-calculate feeds and speeds — you must set them manually
or use a separate speeds-and-feeds calculator.

A rough starting guide:

```
RPM from surface speed:
  RPM = (surface_speed_m_min × 318.3) / diameter_mm

Feed rate:
  feed_mm_min = RPM × chip_load_mm × number_of_flutes
```

| Material | End mill surface speed (m/min) |
|----------|-------------------------------|
| Aluminium | 200–500 |
| Mild steel | 30–60 |
| Hardwood | 100–200 |
| MDF / Plywood | 100–300 |
| HDPE / Acrylic | 150–400 |

!!! warning "Always verify feeds/speeds against your tool manufacturer's data"
    The values above are starting approximations. Tool geometry, coolant,
    machine rigidity, and material grade all affect safe cutting parameters.

---

## Common mistakes and pitfalls

!!! warning "All operations use the same tool"
    Each operation's **Tool Controller** property defaults to the first tool
    controller when the operation is created. Explicitly set the Tool Controller
    for every operation.

!!! warning "Spindle speed is zero in the G-code"
    The Tool Controller's `SpindleSpeed` defaults to 0. Always set a non-zero
    RPM value before running the post-processor.

---

## See also

- [Tool Bit Library](tool-bit-library.md) — manage tool bit definitions
- [Tool Bit Dock](tool-bit-dock.md) — compact floating tool panel
- [New Job](new-job.md) — job setup (tool controllers are part of the job)
- [CAM Workbench](../index.md) — workbench overview
