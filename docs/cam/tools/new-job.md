# New Job

> **In one sentence:** Create a CAM Job — the top-level container that holds
> the model reference, stock, tool controllers, and ordered list of operations
> for a machining programme.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → New Job  
**Command:** `CAM_Job`  
**Shortcut:** none

Creates a new `CAM::Job` object. All machining operations, tool controllers,
and stock definitions are children of the Job.

---

## Intuition

The Job is the starting point for every CNC programme in FreeCAD CAM. Think
of it as the workorder: it says which model to machine, what stock it starts
from, which tools are available, and which operations to run in which order.
You must create a Job before you can add any operations.

---

## When to use it

- Starting any new CNC machining programme.
- Setting up a different fixturing configuration for the same part.
- Creating a second Job for a different post-processor or machine.

---

## Step-by-step

1. Have a Part Design body or Part solid in the document.
2. Select the solid in the model tree.
3. Choose **CAM → New Job**.
4. In the Job task panel:
   - **Model** — the solid(s) to machine (pre-filled from selection)
   - **Stock** — choose stock type (see below)
   - **Post processor** — select the G-code dialect for your machine
5. Click **OK**. The Job object appears in the document tree.

---

## Parameters

### Stock types

| Type | Description |
|------|-------------|
| Create Box | Axis-aligned box auto-sized to model bounding box + extension |
| Create Cylinder | Cylindrical stock (for bar stock) |
| Extend Model's Bounding Box | Box with explicit X, Y, Z oversize values |
| Use Existing Solid | Select an existing FreeCAD shape as the stock |

### Job properties

| Property | Description |
|----------|-------------|
| `PostProcessor` | Name of the post-processor script |
| `PostProcessorArgs` | Arguments passed to the post-processor |
| `PostProcessorOutputFile` | Default output filename for G-code |
| `SplitOutput` | If True, write one file per operation |
| `OrderOutputBy` | Order G-code by: tool, operation, or fixture |

### Setup Sheet

Each Job has a **Setup Sheet** that stores default values for all operations
(feed rate, step down, clearance height, etc.). Operations inherit from the
Setup Sheet but can override any parameter individually.

---

## Common mistakes and pitfalls

!!! warning "Stock is smaller than the model"
    If the auto-computed bounding box has zero extension, the stock coincides
    with the model. Increase the stock extension (extra material beyond the
    model on each face).

!!! warning "Wrong post-processor selected"
    G-code from the wrong post-processor may have incorrect axis letters,
    wrong modal codes, or missing header blocks. Verify the dialect before
    running on the machine.

---

## See also

- [Post Process](post-process.md) — generate G-code from the job
- [Sanity Check](sanity-check.md) — validate job configuration
- [Tool Controller](tool-controller.md) — add tools to the job
- [CAM Workbench](../index.md) — workbench overview
