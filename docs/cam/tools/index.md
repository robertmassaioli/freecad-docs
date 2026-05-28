# CAM Tools

## Job Setup

| Tool | Command | Description |
|------|---------|-------------|
| [New Job](job.md#new-job) | `CAM_Job` | Create a CAM job for the active solid |
| [Post Process](job.md#post-process) | `CAM_Post` | Post-process job to G-code |
| [Sanity Check](job.md#sanity-check) | `CAM_Sanity` | Check job configuration before post-processing |
| [Inspect Path](job.md#inspect-path) | `CAM_Inspect` | View raw G-code for a selected operation |
| [Simulators](job.md#simulate) | `CAM_SimulatorGL` / `CAM_Simulator` | Simulate toolpath material removal |
| [Toggle Active](job.md#toggle-active) | `CAM_OpActiveToggle` | Enable or disable a selected operation |

## 2D Operations

| Tool | Command | Description |
|------|---------|-------------|
| [Profile](operations.md#profile) | `CAM_Profile` | Contour-following pass along edges or faces |
| [Pocket Shape](operations.md#pocket-shape) | `CAM_Pocket_Shape` | 2.5D pocket clearing inside a closed boundary |
| [Face Milling](operations.md#face-milling) | `CAM_MillFace` | Flat face surfacing pass |
| [Helix](operations.md#helix) | `CAM_Helix` | Helical entry into a circular pocket |
| [Adaptive Clearing](operations.md#adaptive-clearing) | `CAM_Adaptive` | Trochoidal adaptive clearing (constant chip load) |
| [Slot](operations.md#slot) | `CAM_Slot` | Slot milling between two points or along an edge |

## 3D Operations

| Tool | Command | Description |
|------|---------|-------------|
| [3D Pocket](operations.md#3d-pocket) | `CAM_Pocket3D` | 3D pocket clearing over a curved floor |
| [Surface](operations.md#surface) | `CAM_Surface` | 3D surface machining (requires OpenCamLib) |
| [Waterline](operations.md#waterline) | `CAM_Waterline` | Horizontal-plane passes at Z levels (requires OpenCamLib) |

## Drilling

| Tool | Command | Description |
|------|---------|-------------|
| [Drilling](operations.md#drilling) | `CAM_Drilling` | Drilling, boring, and peck drilling cycles |

## Engraving

| Tool | Command | Description |
|------|---------|-------------|
| [Engrave](operations.md#engrave) | `CAM_Engrave` | Follow edges or a ShapeString at constant Z |
| [Deburr](operations.md#deburr) | `CAM_Deburr` | Chamfer/deburring pass around edges |
| [V-Carve](operations.md#v-carve) | `CAM_Vcarve` | Variable-depth V-bit carving |

## Setup Operations

| Tool | Command | Description |
|------|---------|-------------|
| [Comment](operations.md#comment) | `CAM_Comment` | Insert a G-code comment |
| [Stop](operations.md#stop) | `CAM_Stop` | Insert a program stop (M0 / M1) |
| [Custom](operations.md#custom) | `CAM_Custom` | Insert arbitrary G-code |
| [Probe](operations.md#probe) | `CAM_Probe` | Touch-off probing routine |

## Dress-ups

| Tool | Command | Description |
|------|---------|-------------|
| [Dogbone](dressup.md#dogbone) | `CAM_DressupDogbone` | Add dogbone/T-bone reliefs at inside corners |
| [Holding Tabs](dressup.md#holding-tabs) | `CAM_DressupTag` | Add material tags to hold a part during cutting |
| [Lead In/Out](dressup.md#lead-inout) | `CAM_DressupLeadInOut` | Add entry/exit arcs to a profile |
| [Ramp Entry](dressup.md#ramp-entry) | `CAM_DressupRampEntry` | Replace plunge entry with a helical or ramp entry |
| [Boundary](dressup.md#boundary) | `CAM_DressupPathBoundary` | Clip the path to a boundary shape |
| [Axis Map](dressup.md#axis-map) | `CAM_DressupAxisMap` | Remap one axis to another |
| [Z Correct](dressup.md#z-correct) | `CAM_DressupZCorrect` | Apply a Z-height correction map |
| [Array](dressup.md#array) | `CAM_DressupArray` | Repeat a path in an array |
| [Drag Knife](dressup.md#drag-knife) | `CAM_DressupDragKnife` | Adapt path for a drag-knife cutter |

## Tool Bit Management

| Tool | Command | Description |
|------|---------|-------------|
| [Tool Bit Library](toolbits.md#library) | `CAM_ToolBitLibraryOpen` | Open the tool bit library manager |
| [Tool Bit Dock](toolbits.md#dock) | `CAM_ToolBitDock` | Show/hide the tool bit dock panel |
| [Tool Controller](toolbits.md#tool-controller) | `CAM_ToolController` | Add a tool controller to the job |
