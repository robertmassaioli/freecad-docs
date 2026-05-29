# Tool Bit Files

> **In one sentence:** A tool bit file (`.fctb`) is a JSON file that stores
> a cutting tool's geometric shape and dimensions — it is the reusable
> description of the physical cutter that gets referenced by Tool Controllers
> in each job.

---

## The tool storage model

FreeCAD CAM separates the concern of *what a tool is* from *how fast to run it*:

| Object | Stores | Reusable across jobs? |
|--------|--------|-----------------------|
| **Tool bit file** (`.fctb`) | Shape, geometry, attributes | Yes — lives in a library on disk |
| **Tool Controller** | Feeds, speeds, tool number | Per-job — references the `.fctb` |

The same 6 mm end mill file can be referenced by multiple Tool Controllers
across multiple Jobs — each with different speeds and feeds for different
materials.

---

## File format

A `.fctb` file is a JSON text file. Example for a 6 mm flat end mill:

```json
{
  "version": 2,
  "name": "6mm EndMill",
  "shape": "endmill.fcstd",
  "parameter": {
    "CuttingEdgeHeight": "20 mm",
    "Diameter": "6 mm",
    "Length": "60 mm",
    "ShankDiameter": "6 mm"
  },
  "attribute": {
    "Flutes": "2",
    "Material": "HSS"
  }
}
```

The `shape` key references an `.fcstd` shape template from FreeCAD's tool
shape library. The `parameter` keys fill in the dimensions of that template.

---

## Tool shapes

FreeCAD ships with shape templates for common cutter types:

| Shape file | Cutter type | Key parameters |
|-----------|-------------|----------------|
| `endmill.fcstd` | Flat-bottom end mill | Diameter, CuttingEdgeHeight, Length, ShankDiameter |
| `ballend.fcstd` | Ball-nose end mill | Diameter, CuttingEdgeHeight, Length, ShankDiameter |
| `bullnose.fcstd` | Corner-radius end mill | Diameter, CuttingEdgeHeight, Length, ShankDiameter, FlatRadius |
| `chamfer.fcstd` | Chamfer / countersink | Diameter, CuttingEdgeAngle, CuttingEdgeHeight, FlatRadius |
| `drill.fcstd` | Twist drill | Diameter, CuttingEdgeHeight, Length, TipAngle |
| `vbit.fcstd` | V-bit / engraving bit | Diameter, CuttingEdgeAngle |
| `slittingsaw.fcstd` | Slitting saw | Diameter, BladeThickness |
| `threadmill.fcstd` | Thread mill | Diameter, Pitch |
| `probe.fcstd` | Touch probe | Diameter (tip sphere) |

---

## Attributes

The `attribute` section stores metadata that does not affect toolpath geometry
but is useful for tool management:

| Attribute | Typical values |
|-----------|---------------|
| `Flutes` | `"2"`, `"3"`, `"4"` |
| `Material` | `"HSS"`, `"Carbide"`, `"Cobalt"` |
| `Coating` | `"TiN"`, `"TiAlN"`, `"DLC"`, `"Uncoated"` |
| `Vendor` | Manufacturer name |
| `Product Url` | Link to product page |

Attributes are not used by the FreeCAD toolpath engine — they are for
reference only.

---

## Storage locations

Tool bit files are stored on disk in a library directory:

| Platform | User library path |
|----------|------------------|
| Linux | `~/.local/share/FreeCAD/Tools/Bit/` |
| Windows | `%APPDATA%\FreeCAD\Tools\Bit\` |
| macOS | `~/Library/Application Support/FreeCAD/Tools/Bit/` |

FreeCAD also ships sample libraries in `<install_dir>/Mod/CAM/Tools/Library/`.

Libraries are `.fctl` JSON files that simply list the paths to `.fctb` files.
Multiple libraries can be active at once.

---

## Creating and editing tool bits

### Via the Tool Bit Library manager

1. **CAM → Tool Bit Library** — opens the Library manager.
2. Click **New Tool** — a wizard prompts for shape and dimensions.
3. Fill in all parameters and click **OK**.
4. The new `.fctb` file is saved in the active library directory.

### Manually

Create a `.fctb` JSON file with a text editor. Copy an existing file as a
starting template, change the `parameter` values, and save with a descriptive
name like `6mm-2flute-carbide-endmill.fctb`.

---

## Adding a tool to a Job

After the tool bit file exists in a library:

1. Open the Library manager (**CAM → Tool Bit Library**).
2. Select the tool in the list.
3. Click **Add to Job** (or drag onto the Job in the model tree).
4. A new **Tool Controller** appears in the Job — set `HorizFeed`, `VertFeed`,
   and `SpindleSpeed` to appropriate values for the material.

---

## Common mistakes and pitfalls

!!! warning "Tool bit file not found after moving libraries"
    The Library manager stores absolute file paths to `.fctb` files. If you
    move the library directory, the references break. Use
    **right-click → Update path** in the Library manager to fix them, or
    keep all tool files in the user's standard library directory.

!!! warning "Diameter is the key parameter for most toolpaths"
    Pocket, Profile, and other operations use the `Diameter` parameter to
    compute the toolpath offset. If the diameter is wrong in the `.fctb`,
    the toolpath offset will be wrong — and the finished part will be
    under- or over-sized.

!!! warning "Shape template must exist"
    The `shape` key in the `.fctb` must point to a valid `.fcstd` shape
    template. Misspelling the shape name (e.g. `"endMill.fcstd"` instead of
    `"endmill.fcstd"`) causes the tool bit to fail to load.

!!! warning "Attributes do not affect toolpaths"
    The `Flutes` attribute is metadata — the CAM engine does not use it to
    adjust chip load or feeds. You must use `Flutes` yourself when calculating
    feed rates (see [Feeds and Speeds](feeds-and-speeds.md)).

---

## Python API

```python
import Path.Tool.Bit as ToolBit
import Path.Tool.Controller as ToolController
import FreeCAD as App

# Load a tool bit from a file
tool = ToolBit.ToolBit()
tool.loadFrom("/path/to/6mm-endmill.fctb")
print(tool.Parameter["Diameter"])   # "6 mm"

# Create a Tool Controller for this bit
doc = App.ActiveDocument
job = doc.getObject("Job")

tc = ToolController.Create("6mmEndMill")
tc.Tool = tool
tc.HorizFeed = 1600    # mm/min
tc.VertFeed  = 500     # mm/min
tc.SpindleSpeed = 16000  # RPM
tc.SpindleDir = 0       # 0 = CW
job.addObject(tc)
doc.recompute()
```

---

## See also

- [Tool Bit Library](../tools/tool-bit-library.md) — graphical tool management
- [Tool Bit Dock](../tools/tool-bit-dock.md) — compact floating tool panel
- [Tool Controller](../tools/tool-controller.md) — feeds, speeds, and tool number per job
- [Feeds and Speeds](feeds-and-speeds.md) — how to calculate correct cutting parameters
- [CAM Workbench](../index.md) — workbench overview
