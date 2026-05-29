# Common Operation Parameters

> **Reference:** Depth, height, and tool controller parameters shared by
> all CAM operations.

---

## Overview

Every CAM operation has a common set of depth and height parameters that
control how far the tool cuts and how it moves between cuts. These parameters
are set per-operation or inherited from the Job's **Setup Sheet**.

---

## Depths

| Parameter | Description |
|-----------|-------------|
| Start Depth | Z level where cutting begins (top of the feature) |
| Final Depth | Z level at the bottom of the cut |
| Step Down | Maximum depth of cut per Z-level pass |

The tool moves from **Start Depth** down to **Final Depth** in steps of
**Step Down**. A smaller Step Down means more passes but less load per pass.

**Rule of thumb for Step Down:** 25–50 % of the tool diameter for end mills
in soft materials; 10–25 % for hard materials or small tools.

---

## Heights

| Parameter | Description |
|-----------|-------------|
| Clearance Height | Z level for rapid moves between separate cut regions |
| Safe Height | Z level for repositioning above the stock between operations |

**Clearance Height** must be above all stock features and clamps.
**Safe Height** should be at least 5–10 mm above the top of the stock.

---

## Tool Controller

Each operation has a **Tool Controller** property that selects which tool
and feed/speed settings to use for that operation. Tool Controllers are
configured in the Job and reference a tool bit from the library.

See [Tool Controller](tool-controller.md) for details.

---

## Setup Sheet inheritance

Operations inherit default parameter values from the Job's **Setup Sheet**
but can override any parameter individually. If you change a parameter in
the Setup Sheet after operations have been created, operations that have
not been overridden will update automatically.

---

## See also

- [New Job](new-job.md) — configure the Setup Sheet defaults
- [Tool Controller](tool-controller.md) — assign tools to operations
- [All Operations](index.md) — list of all machining operations
