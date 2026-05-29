# Operation Parameters

> **In one sentence:** Every CAM operation shares a common set of depth,
> height, and tool controller parameters that control how far the cutter
> cuts and how it moves between cuts — understanding these is the foundation
> of safe, efficient toolpath generation.

---

## The parameter hierarchy

Parameters flow from broad to specific:

```
Job → Setup Sheet (defaults for the whole job)
       └── Operation (can override any Setup Sheet value)
                └── Dress-up (post-processing modifier applied on top)
```

When you change a value in the Setup Sheet, all operations that have not
been individually overridden update automatically. Operations with a manual
override keep their own value.

---

## Depth parameters

Depth parameters control the Z range of the cut. All operations share these.

| Parameter | Meaning | Typical source |
|-----------|---------|----------------|
| **Start Depth** | Z level where the tool first contacts the stock | Top face of the stock, or a step above a previous pass |
| **Final Depth** | Z level at the bottom of the cut | Bottom of the pocket, or bottom of the stock for through-cuts |
| **Step Down** | Maximum depth removed per Z-level pass | 25–50 % of tool diameter (soft); 10–25 % (hard) |

The operation generates passes at `Start Depth`, `Start Depth − StepDown`,
`Start Depth − 2×StepDown`, … down to `Final Depth`. The last pass always
lands exactly at `Final Depth` even if the final step is smaller than
`StepDown`.

### Choosing Step Down

| Material | Recommended Step Down |
|----------|----------------------|
| Softwood, MDF, foam | 50–100 % of diameter |
| Hardwood, aluminium (large tool) | 25–50 % of diameter |
| Aluminium (small tool < 6 mm) | 10–25 % of diameter |
| Mild steel | 5–15 % of diameter |
| Stainless / hard alloys | 3–10 % of diameter |

A smaller Step Down means more passes but less cutting force per pass.
Underpowered machines or long-reach tools require smaller Step Down to
avoid deflection and breakage.

---

## Height parameters

Height parameters control rapid (non-cutting) Z positions.

| Parameter | Meaning | Typical value |
|-----------|---------|---------------|
| **Safe Height** | Z level for rapid repositioning over the stock surface | 5–10 mm above top of stock |
| **Clearance Height** | Z level for rapid moves between separate cut regions | Above all clamps and fixtures |

The tool moves to **Clearance Height** between separate cut regions (e.g.
between two pockets on the same face). It moves to **Safe Height** for
shorter repositions within a region where no obstacle exists between the
current and next position.

!!! warning "Clearance Height must clear all clamps"
    Measure your tallest clamp and set Clearance Height above it. A rapid
    move at Safe Height can crash into a clamp if only Safe Height is used
    and it is too low.

!!! warning "Both heights are absolute Z, not relative to stock"
    If the stock is at Z = 50 mm (mounted on a riser), a Clearance Height
    of 10 mm would drive the tool into the stock. Heights are in the model's
    coordinate system — add the stock elevation.

---

## Tool controller

Every operation has a **Tool Controller** property that assigns:

- Which tool bit (geometry — diameter, shape, flute count) to use.
- The feed rates and spindle speed for that tool.

| Property | Description |
|----------|-------------|
| Tool | Reference to a `.fctb` tool bit definition |
| HorizFeed | Cutting feed rate for XY motion (mm/min) |
| VertFeed | Plunge feed rate for Z motion (mm/min) |
| SpindleSpeed | Spindle speed (RPM) |
| SpindleDir | Clockwise or counter-clockwise |

See [Tool Controller](../tools/tool-controller.md) for detailed setup.
See [Feeds and Speeds](feeds-and-speeds.md) for how to calculate correct values.

!!! warning "Default Tool Controller selection"
    When you add an operation, it defaults to the first Tool Controller in
    the Job. Always verify the correct tool is assigned before generating
    the toolpath.

---

## Operation-specific parameters

Beyond the common parameters, each operation type has its own settings.
Examples:

| Operation | Key specific parameters |
|-----------|------------------------|
| [Profile](../tools/profile.md) | Side of cut (inside/outside), offset distance |
| [Pocket](../tools/pocket-shape.md) | Pattern (zigzag, offset, spiral), step-over |
| [Drilling](../tools/drilling.md) | Drill cycle type (G81/G82/G83), peck depth, dwell |
| [Adaptive Clearing](../tools/adaptive-clearing.md) | Step-over, lift height |
| [Engraving](../tools/engrave.md) | Start depth only (engraving is always shallow) |
| [Face Milling](../tools/face-milling.md) | Step-over, boundary extension |

---

## Setup Sheet inheritance

The Job's **Setup Sheet** stores default values for all the shared parameters.
When you create an operation, it inherits Setup Sheet values. You can
override any parameter per-operation.

To reset an overridden parameter back to the Setup Sheet default:
right-click the parameter in the operation's Properties panel and choose
**Reset to default** (or delete the per-operation value).

```
Setup Sheet: Final Depth = −10 mm, Step Down = 2 mm, Safe Height = 15 mm
Operation 1:  (uses all Setup Sheet defaults)
Operation 2:  Final Depth overridden to −5 mm  (other params still inherited)
```

---

## Python API

```python
import FreeCAD as App
import Path

doc = App.ActiveDocument
job = doc.getObject("Job")

# Access an operation's properties
op = doc.getObject("Profile")
print(op.StartDepth)      # App.Units.Quantity
print(op.FinalDepth)
print(op.StepDown)
print(op.SafeHeight)
print(op.ClearanceHeight)

# Change a parameter
op.FinalDepth = App.Units.Quantity("-15 mm")
op.StepDown   = App.Units.Quantity("1.5 mm")

doc.recompute()
```

---

## See also

- [Feeds and Speeds](feeds-and-speeds.md) — how to calculate RPM and feed rates
- [Tool Bit Files](tool-bit-files.md) — how tool geometry is stored
- [Tool Controller](../tools/tool-controller.md) — assign tools and parameters to operations
- [New Job](../tools/new-job.md) — job setup and Setup Sheet
- [Common Parameters reference](../tools/common-parameters.md) — quick reference table
- [CAM Workbench](../index.md) — workbench overview
