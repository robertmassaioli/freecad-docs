# Sanity Check

> **In one sentence:** Validate the Job configuration — checking that models,
> stock, tool controllers, depths, and operations are all correctly set before
> post-processing.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Sanity Check  
**Command:** `CAM_Sanity`  
**Shortcut:** none

Runs a series of validation checks on the active Job and reports warnings and
errors. Warnings indicate potential issues; errors must be resolved before
post-processing will produce correct output.

---

## Intuition

Running a faulty programme on a CNC machine risks breaking tools, damaging
the workpiece, or crashing the machine. Sanity Check is a quick automated
review that catches the most common configuration errors before you generate
the G-code file.

Always run Sanity Check before Post Process.

---

## Checks performed

| Check | What is verified |
|-------|-----------------|
| Model present | At least one model is referenced |
| Stock defined | Stock shape is valid and larger than the model |
| Tool controllers | All operations have a valid tool controller assigned |
| Depths valid | Final depth is below start depth; step down > 0 |
| No empty operations | All active operations produce at least one path segment |

---

## Step-by-step

1. With the Job selected, choose **CAM → Sanity Check**.
2. Review the report — green = OK, yellow = warning, red = error.
3. Fix all errors and re-run before post-processing.

---

## See also

- [Post Process](post-process.md) — run after passing sanity check
- [New Job](new-job.md) — configure the job correctly
- [CAM Workbench](../index.md) — workbench overview
