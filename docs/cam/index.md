# CAM Workbench

> **In one sentence:** The CAM workbench generates CNC toolpaths from a
> 3-D model — you set up a Job, add machining operations (profile, pocket,
> drilling, engraving), apply dress-ups (tabs, lead-ins, dogbone corners),
> simulate the result, and post-process to G-code for your machine.

---

## What is the CAM workbench for?

CAM (**Computer-Aided Manufacturing**, formerly called Path) produces
machine-readable G-code from FreeCAD geometry. It supports:

- **2.5D milling** — profiling, pocketing, face milling, slot milling,
  adaptive clearing, helix drilling.
- **3D machining** — 3-D pocket, surface machining, and waterline passes
  (the last two require the optional OpenCamLib library).
- **Drilling** — drilling, boring, and (experimental) tapping cycles.
- **Engraving and V-carving** — following edges, text, or artwork at
  varying depths.
- **Dress-ups** — post-processing modifiers applied on top of operations:
  holding tabs, lead-in/out arcs, dogbone corner compensation, ramping entry.

The output is **machine-neutral** — a post-processor script converts the
internal path representation to the G-code dialect of a specific controller
(Grbl, LinuxCNC, Mach3, Fanuc, etc.).

---

## The Job — the central document object

Everything in the CAM workbench lives inside a **Job** object
(`Path::Job` / `CAM_Job`). The Job holds:

- A reference to the **model** (the Part Design body or solid to be machined).
- **Stock** — the raw material block (auto-sized bounding box or custom).
- **Tool Controller** list — which tool bits are used, with feed/speed.
- **Setup Sheet** — default feeds, speeds, and depths for the job.
- **Operations** — the list of machining operations, in execution order.

The Job is created first; all operations are added to it. G-code is produced
by running **Post Process** (`CAM_Post`) which calls the selected post-processor
script.

---

## Concepts

### Coordinate system

CAM uses the same coordinate system as the FreeCAD model. The **origin** is
typically at the bottom-left-front corner of the stock, or at the top-centre
of the stock (depending on the post-processor configuration). Always check
your machine's work coordinate system (WCS) matches.

### Tool Controllers

A **Tool Controller** (`CAM_ToolController`) connects a tool bit to a set
of parameters:

| Parameter | Description |
|-----------|-------------|
| Tool | Reference to a tool bit definition |
| Feed rate (horizontal) | XY cutting feed in mm/min |
| Feed rate (vertical) | Z plunge feed in mm/min |
| Spindle speed | RPM |
| Spindle direction | CW / CCW |

Each operation can be assigned a specific Tool Controller from the job's list.

### Stock

The **Stock** defines the workpiece material envelope. It can be:
- **Box stock** — auto-computed bounding box of the model + an offset.
- **Cylinder stock** — circular bar stock.
- **Extended box** — manual dimensions.
- **From existing solid** — use an existing FreeCAD shape.

### Depths and heights

Each operation has depth parameters:

| Parameter | Meaning |
|-----------|---------|
| Start Depth | Z level where the tool first engages |
| Final Depth | Z level at the bottom of the cut |
| Step Down | Maximum depth of cut per pass |
| Clearance Height | Z level for rapid moves between cuts |
| Safe Height | Z level for rapid moves over stock |

---

## Workflow overview

```
Create Part Design solid
        │
        ▼
CAM → New Job (select solid, configure stock)
        │
        ▼
Add Tool Controllers (select tool bits, set feeds/speeds)
        │
        ▼
Add Operations (Profile, Pocket, Drilling, …)
  for each operation:
    ├── Select geometry (faces, edges, holes)
    ├── Set depths (Start Depth, Final Depth, Step Down)
    └── Configure operation-specific parameters
        │
        ▼
Apply Dress-ups (Tabs, LeadInOut, Dogbone, RampEntry, …)
        │
        ▼
Simulate (GL Simulator or legacy Simulator)
        │
        ▼
Post Process → G-code file
```

---

## Post-processors

FreeCAD ships with post-processors for common controllers:

- **grbl** — for Grbl-based routers and laser cutters
- **linuxcnc** — LinuxCNC / EMC2
- **mach3_milling**, **mach4_milling** — Mach3/Mach4
- **centroid** — Centroid CNC controllers
- **fanuc** — Fanuc-compatible controllers
- **smoothie** — Smoothieware
- Many others in `Path/Post/scripts/`

Post-processors are Python scripts in `Mod/CAM/Path/Post/scripts/`. Custom
post-processors can be added to the user's macro directory.

---

## See also

- [New Job](tools/new-job.md) — Job, Post Process, Sanity, Tool Controllers, Stock
- [All Operations](tools/index.md) — all machining operations
- [Dress-ups](tools/dressup-dogbone.md) — path modifiers (tabs, lead-ins, dogbone…)
- [Tool Bits and Libraries](tools/tool-bit-library.md) — tool bit management
