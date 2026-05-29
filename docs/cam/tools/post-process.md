# Post Process

> **In one sentence:** Run the selected post-processor on the active Job
> and write the G-code output to a file.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Post Process  
**Command:** `CAM_Post`  
**Shortcut:** none

Calls the post-processor Python script for the active Job and writes the
G-code to a file. The post-processor translates FreeCAD's internal path
representation to the G-code dialect of a specific CNC controller.

---

## Intuition

FreeCAD stores toolpaths in a machine-neutral internal format (always metric,
always absolute coordinates). Post Process applies a post-processor script —
a Python file that knows your machine's specific dialect — to convert this to
the G-code your controller actually accepts.

---

## When to use it

- When you are ready to cut and need the G-code output file.
- When testing post-processor output during setup.

## When NOT to use it

- **Don't skip Sanity Check first** — run [Sanity Check](sanity-check.md) to
  catch configuration errors before post-processing.

---

## Step-by-step

1. Run [Sanity Check](sanity-check.md) to verify the job.
2. With the Job selected (or active), choose **CAM → Post Process**.
3. A file dialog opens — choose the output path and filename.
4. Click **Save**. FreeCAD runs the post-processor and writes the G-code.

---

## Post-processor arguments

Post-processors accept command-line style arguments in the Job's
**PostProcessorArgs** field:

| Argument | Effect |
|----------|--------|
| `--no-show-editor` | Skip the G-code preview editor |
| `--inches` | Output in inches instead of mm |
| `--comments` | Include human-readable comments |
| `--header` | Include file header block |

Arguments vary by post-processor. Run the post-processor with `--help` to
see all available options.

---

## Common mistakes and pitfalls

!!! warning "Output file is empty"
    All operations may be disabled, or no operations produce a valid path.
    Run [Sanity Check](sanity-check.md) and verify at least one operation
    is active.

!!! warning "G-code coordinates are wrong scale"
    The post-processor may be outputting in inches while the machine expects
    mm, or vice versa. Check the `--inches` argument and verify with
    `G20`/`G21` in the output file header.

---

## See also

- [Sanity Check](sanity-check.md) — validate before post-processing
- [New Job](new-job.md) — configure the post-processor on the job
- [Simulate](simulate.md) — preview before post-processing
- [CAM Workbench](../index.md) — workbench overview
