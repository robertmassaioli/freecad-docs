# Tool Bit Library

> **In one sentence:** Open the tool library manager to browse, create, and
> manage all cutting tool definitions for your jobs.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Tool Bit Library  
**Command:** `CAM_ToolBitLibraryOpen`  
**Shortcut:** none

Opens the Tool Bit Library manager — a panel that shows all tool libraries
and the tools in each. From here you can create new tools, edit existing ones,
and add tools to the active Job as Tool Controllers.

---

## Intuition

FreeCAD stores cutting tools as `.fctb` JSON files on disk, organised into
libraries. The Library manager is the main interface for managing this
collection — think of it as a file browser for your tool cabinet.

---

## Tool bit files

A tool bit file (`.fctb`) contains:

- **Shape** — which template to use (end mill, ball end, V-bit, drill, chamfer, etc.)
- **Geometry** — dimensions (diameter, flute length, shaft diameter, tip angle, etc.)
- **Attributes** — number of flutes, coating, material

### Default tool shapes

| Shape | Description |
|-------|-------------|
| EndMill | Flat-bottom end mill |
| BallEnd | Ball-nose end mill for 3D surfacing |
| BullNose | End mill with a corner radius |
| Chamfer | Countersink or chamfer cutter |
| Drill | Twist drill |
| SlittingSaw | Slitting saw for slot cutting |
| Vbit | V-shaped engraving or V-carve bit |
| ThreadMill | Thread milling cutter |
| ProbeToolBit | Measurement probe |

### Storage locations

| Platform | Path |
|----------|------|
| Linux | `~/.local/share/FreeCAD/Tools/Bit/` |
| Windows | `%APPDATA%\FreeCAD\Tools\Bit\` |
| macOS | `~/Library/Application Support/FreeCAD/Tools/Bit/` |

FreeCAD also ships sample libraries in `Mod/CAM/Tools/Library/`.

---

## Operations in the Library manager

| Action | Description |
|--------|-------------|
| New Tool | Create a new tool bit using the wizard |
| Delete Tool | Remove a tool from the library |
| Import Tool | Import a `.fctb` file from disk |
| Export Tool | Export a tool to a `.fctb` file |
| Add to Job | Add the selected tool as a Tool Controller to the active Job |

---

## Common mistakes and pitfalls

!!! warning "Tool bit not found after moving files"
    The `.fctb` file path stored in the library has changed. In the Library
    manager, right-click the broken tool and update its file path, or
    recreate the tool bit.

---

## See also

- [Tool Bit Dock](tool-bit-dock.md) — compact floating panel for quick tool access
- [Tool Controller](tool-controller.md) — configure feeds, speeds, and tool number
- [CAM Workbench](../index.md) — workbench overview
