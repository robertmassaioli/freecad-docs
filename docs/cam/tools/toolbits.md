# Tool Bits and Libraries

FreeCAD's CAM workbench uses a **tool bit asset system** to manage cutting
tools. Each tool is stored as a JSON file containing its geometry and
parameters. Tools are organised into **libraries** (collections of tools for
a particular job or machine). A **Tool Controller** connects a library tool
to an operation with its specific feeds and speeds.

---

## Tool bit files

A tool bit is a `.fctb` file (JSON format) stored in the user's tool bit
directory (`~/.local/share/FreeCAD/Tools/Bit/` on Linux,
`%APPDATA%\FreeCAD\Tools\Bit\` on Windows).

The file contains:
- **Shape** — which template shape to use (end mill, ball end, V-bit, drill,
  chamfer, slitting saw, etc.)
- **Geometry** — dimensions (diameter, flute length, shaft diameter, angle…)
- **Attributes** — number of flutes, coating, material

### Tool shapes (templates)

| Shape | Description |
|-------|-------------|
| `EndMill` | Flat-bottom end mill |
| `BallEnd` | Ball-nose end mill for 3D surfacing |
| `BullNose` | Bull-nose end mill (flat with radius corner) |
| `Chamfer` | Countersink / chamfer cutter |
| `Drill` | Twist drill |
| `SlittingSaw` | Slitting saw for slot cutting |
| `Vbit` | V-shaped engraving/V-carve bit |
| `ThreadMill` | Thread milling cutter |
| `ProbeToolBit` | Measurement probe |

---

## Tool Bit Library {#library}

**Menu:** CAM → Tool Bit Library  
**Command:** `CAM_ToolBitLibraryOpen`

Opens the **Tool Bit Library** manager — a panel showing all tool libraries
and the tools in each.

### Operations in the Library manager

| Action | Description |
|--------|-------------|
| **New Tool** | Create a new tool bit using the tool bit creator wizard |
| **Delete Tool** | Remove a tool from the library |
| **Import Tool** | Import a `.fctb` file from disk |
| **Export Tool** | Export a tool to a `.fctb` file |
| **Add to Job** | Add the selected tool as a Tool Controller to the active Job |

### Default libraries

FreeCAD ships with sample tool libraries in:
`Mod/CAM/Tools/Library/`

These include common end mills, drills, and V-bits. Modify the sample
libraries or create your own.

---

## Tool Bit Dock {#dock}

**Menu:** CAM → Tool Bit Dock  
**Command:** `CAM_ToolBitDock`

Shows or hides the **Tool Bit Dock** — a compact floating panel for quickly
adding tools to a job without opening the full library manager.

---

## Tool Controller {#tool-controller}

**Command:** `CAM_ToolController`

A **Tool Controller** (`CAM::ToolController`) is the bridge between a tool
bit definition and a Job operation. It stores:

| Property | Description |
|----------|-------------|
| `Tool` | Reference to the tool bit |
| `HorizFeed` | Horizontal cutting feed rate (mm/min) |
| `VertFeed` | Vertical (plunge) feed rate (mm/min) |
| `SpindleSpeed` | Spindle speed (RPM) |
| `SpindleDir` | Clockwise or counter-clockwise |

### Adding a Tool Controller

1. Open the Library manager or Tool Bit Dock.
2. Select a tool from the library.
3. Click **Add to Job** or drag the tool onto the Job.
4. A new Tool Controller appears in the Job's tool list.
5. Set the feed/speed values in the Tool Controller properties.

### Assigning a Tool Controller to an operation

Each operation has a **Tool Controller** property. Click the dropdown and
select from the job's tool controller list.

---

## Calculating feeds and speeds

FreeCAD does **not** auto-calculate feeds and speeds — you must set them
manually or use a separate speeds-and-feeds calculator.

A rough starting guide:

```
Surface speed (SFM to RPM): RPM = (SFM × 3.82) / Diameter_inches
                     m/min: RPM = (m/min × 318.3) / Diameter_mm

Feed rate (mm/min) = RPM × chip_load_mm × number_of_flutes
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

!!! warning "Tool bit not found after moving files"
    **Cause:** The `.fctb` file path in the library has changed.  
    **Fix:** In the Library manager, right-click the broken tool and update
    its file path. Or recreate the tool bit.

!!! warning "All operations use the same tool even though different tools are selected"
    **Cause:** Each operation's **Tool Controller** property is still set to
    the first tool controller from when the operation was created.  
    **Fix:** In each operation's properties, explicitly set the Tool
    Controller to the desired one.

!!! warning "Spindle speed is zero in the G-code"
    **Cause:** The Tool Controller's `SpindleSpeed` is 0 (the default).  
    **Fix:** Set a non-zero RPM value in the Tool Controller's properties.
