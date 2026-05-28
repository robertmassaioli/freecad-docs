# Job Setup

---

## New Job {#new-job}

**Menu:** CAM → New Job  
**Command:** `CAM_Job`

Creates a new CAM Job — the top-level container for all machining data for
a model. A Job holds the stock, tool controllers, setup sheet, and operations.

### Step-by-step

1. Have a Part Design body or Part solid in the document.
2. Select the solid.
3. Go to **CAM → New Job** (or click the Job button in the toolbar).
4. The **Job** task panel opens:
   - **Model** — the solid(s) to machine (pre-filled from selection)
   - **Stock** — choose stock type (auto bounding box, custom box, cylinder)
   - **Post processor** — select the G-code dialect for your machine

5. Click **OK**. The Job object appears in the document tree.

### Stock types

| Type | Description |
|------|-------------|
| **Create Box** | Axis-aligned box auto-sized to model bounding box + extension |
| **Create Cylinder** | Cylindrical stock (bar stock) |
| **Extend Model's Bounding Box** | Box with explicit X, Y, Z oversize values |
| **Use Existing Solid** | Select an existing shape as the stock |

### Setup Sheet

Each Job has a **Setup Sheet** that stores default values for all operations:
- Default feed rate, plunge rate, spindle speed
- Default final depth, step down, clearance height

Operations inherit from the Setup Sheet but can override any parameter
individually. Changing the Setup Sheet updates all non-overridden operations.

### Job properties

| Property | Description |
|----------|-------------|
| `PostProcessor` | Name of the post-processor script |
| `PostProcessorArgs` | Arguments passed to the post-processor |
| `PostProcessorOutputFile` | Default output filename for G-code |
| `SplitOutput` | If True, write one file per operation |
| `OrderOutputBy` | Order G-code by: tool, operation, or fixture |

---

## Post Process {#post-process}

**Menu:** CAM → Post Process  
**Command:** `CAM_Post`

Runs the selected post-processor on the Job and writes G-code to a file.

### Step-by-step

1. With a Job selected (or active), go to **CAM → Post Process**.
2. A file dialog opens. Choose the output filename and location.
3. FreeCAD calls the post-processor Python script and writes the G-code file.

### Post-processor arguments

Post-processors accept command-line style arguments in the **PostProcessorArgs**
field. Common arguments:

| Argument | Effect |
|----------|--------|
| `--no-show-editor` | Skip the G-code preview editor |
| `--inches` | Output in inches instead of mm |
| `--comments` | Include human-readable comments |
| `--header` | Include file header block |

Arguments vary by post-processor. Refer to the post-processor's `--help`
output for the full list.

---

## Sanity Check {#sanity-check}

**Menu:** CAM → Sanity Check  
**Command:** `CAM_Sanity`

Validates the Job configuration before post-processing. Checks include:

| Check | What is verified |
|-------|-----------------|
| **Model present** | At least one model is referenced |
| **Stock defined** | Stock shape is valid and larger than the model |
| **Tool controllers** | All operations have a valid tool controller assigned |
| **Depths valid** | Final depth is below start depth; step down > 0 |
| **No empty operations** | All operations produce at least one path segment |

Results are shown in a report. Warnings indicate potential issues; errors
must be resolved before post-processing.

---

## Inspect Path {#inspect-path}

**Menu:** CAM → Inspect Path  
**Command:** `CAM_Inspect`

Displays the raw internal G-code representation of the selected operation.
This is the intermediate G-code before post-processing — it uses FreeCAD's
canonical G-code format (always metric, always absolute).

Use this to debug operations that produce unexpected toolpaths.

---

## Simulate {#simulate}

**Menu:** CAM → Simulators  
**Commands:** `CAM_SimulatorGL`, `CAM_Simulator`

### GL Simulator (default)

The GL Simulator renders the material removal in real time using OpenGL.
The stock starts as a solid block and the tool path is animated, removing
material as the virtual tool moves.

| Control | Action |
|---------|--------|
| Play / Pause | Start/stop animation |
| Speed slider | Playback speed multiplier |
| Step | Advance one G-code line |
| Show tool | Toggle tool mesh visibility |

### Legacy Simulator

The older Python-based simulator (slower, but works without OpenGL). It
shows the resulting mesh after all operations are applied.

---

## Toggle Active {#toggle-active}

**Menu:** (context menu) → Toggle Active  
**Command:** `CAM_OpActiveToggle`

Enables or disables the selected operation in the Job. A disabled operation
is skipped during post-processing and simulation but remains in the document.

Use this to temporarily exclude operations during development (e.g. skip
the facing pass when testing a finishing pass).

---

## Common mistakes and pitfalls

!!! warning "Post Process produces an empty file"
    **Cause:** All operations are disabled, or the Job has no operations.  
    **Fix:** Run Sanity Check. Ensure at least one operation is active and
    produces a valid path.

!!! warning "G-code coordinates are wrong scale"
    **Cause:** The post-processor is outputting in inches but the machine
    expects mm, or vice versa.  
    **Fix:** Check the `--inches` argument and the post-processor's unit
    handling. Verify with `G20`/`G21` in the output file header.

!!! warning "Stock is smaller than the model"
    **Cause:** The auto-computed stock bounding box has zero extension.  
    **Fix:** In the Job setup, increase the stock extension (the extra
    material on each face beyond the model).
