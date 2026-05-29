# CAM Tools

## Job Setup

| Tool | Command | Description |
|------|---------|-------------|
| [New Job](new-job.md) | `CAM_Job` | Create a CAM job for the active solid |
| [Post Process](post-process.md) | `CAM_Post` | Post-process job to G-code |
| [Sanity Check](sanity-check.md) | `CAM_Sanity` | Check job configuration before post-processing |
| [Inspect Path](inspect-path.md) | `CAM_Inspect` | View raw G-code for a selected operation |
| [Simulate](simulate.md) | `CAM_SimulatorGL` / `CAM_Simulator` | Simulate toolpath material removal |
| [Toggle Active](toggle-active.md) | `CAM_OpActiveToggle` | Enable or disable a selected operation |

## 2D Operations

| Tool | Command | Description |
|------|---------|-------------|
| [Profile](profile.md) | `CAM_Profile` | Contour-following pass along edges or faces |
| [Pocket Shape](pocket-shape.md) | `CAM_Pocket_Shape` | 2.5D pocket clearing inside a closed boundary |
| [Face Milling](face-milling.md) | `CAM_MillFace` | Flat face surfacing pass |
| [Helix](helix.md) | `CAM_Helix` | Helical entry into a circular pocket |
| [Adaptive Clearing](adaptive-clearing.md) | `CAM_Adaptive` | Trochoidal adaptive clearing (constant chip load) |
| [Slot](slot.md) | `CAM_Slot` | Slot milling between two points or along an edge |

## 3D Operations

| Tool | Command | Description |
|------|---------|-------------|
| [3D Pocket](3d-pocket.md) | `CAM_Pocket3D` | 3D pocket clearing over a curved floor |
| [Surface](surface.md) | `CAM_Surface` | 3D surface machining (requires OpenCamLib) |
| [Waterline](waterline.md) | `CAM_Waterline` | Horizontal-plane passes at Z levels (requires OpenCamLib) |

## Drilling

| Tool | Command | Description |
|------|---------|-------------|
| [Drilling](drilling.md) | `CAM_Drilling` | Drilling, boring, and peck drilling cycles |

## Engraving

| Tool | Command | Description |
|------|---------|-------------|
| [Engrave](engrave.md) | `CAM_Engrave` | Follow edges or a ShapeString at constant Z |
| [Deburr](deburr.md) | `CAM_Deburr` | Chamfer/deburring pass around edges |
| [V-Carve](v-carve.md) | `CAM_Vcarve` | Variable-depth V-bit carving |

## Setup Operations

| Tool | Command | Description |
|------|---------|-------------|
| [Comment](comment.md) | `CAM_Comment` | Insert a G-code comment |
| [Stop](stop.md) | `CAM_Stop` | Insert a program stop (M0 / M1) |
| [Custom](custom.md) | `CAM_Custom` | Insert arbitrary G-code |
| [Probe](probe.md) | `CAM_Probe` | Touch-off probing routine |

## Dress-ups

| Tool | Command | Description |
|------|---------|-------------|
| [Dogbone](dressup-dogbone.md) | `CAM_DressupDogbone` | Add dogbone/T-bone reliefs at inside corners |
| [Holding Tabs](dressup-holding-tabs.md) | `CAM_DressupTag` | Add material tags to hold a part during cutting |
| [Lead In/Out](dressup-lead-inout.md) | `CAM_DressupLeadInOut` | Add entry/exit arcs to a profile |
| [Ramp Entry](dressup-ramp-entry.md) | `CAM_DressupRampEntry` | Replace plunge entry with a helical or ramp entry |
| [Boundary](dressup-boundary.md) | `CAM_DressupPathBoundary` | Clip the path to a boundary shape |
| [Axis Map](dressup-axis-map.md) | `CAM_DressupAxisMap` | Remap one axis to another |
| [Z Correct](dressup-z-correct.md) | `CAM_DressupZCorrect` | Apply a Z-height correction map |
| [Array](dressup-array.md) | `CAM_DressupArray` | Repeat a path in an array |
| [Drag Knife](dressup-drag-knife.md) | `CAM_DressupDragKnife` | Adapt path for a drag-knife cutter |

## Tool Bit Management

| Tool | Command | Description |
|------|---------|-------------|
| [Tool Bit Library](tool-bit-library.md) | `CAM_ToolBitLibraryOpen` | Open the tool bit library manager |
| [Tool Bit Dock](tool-bit-dock.md) | `CAM_ToolBitDock` | Show/hide the tool bit dock panel |
| [Tool Controller](tool-controller.md) | `CAM_ToolController` | Add a tool controller to the job |

## Concepts

- [Common Parameters](common-parameters.md) — shared depth, height, and tool controller parameters
