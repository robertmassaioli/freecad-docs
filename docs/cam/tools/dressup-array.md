# Dress-up: Array

> **In one sentence:** Repeat the selected operation in a rectangular or
> circular array as a post-processing step.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Array  
**Command:** `CAM_DressupArray`  
**Shortcut:** none

Repeats the toolpath of the selected operation in a rectangular (X/Y grid)
or circular (angular) array. Unlike duplicating operations in the Job tree,
Array applies the repetition as a dress-up — a single lightweight modifier
on the base operation.

---

## Intuition

When a Job contains multiple identical parts laid out in a grid, you could
duplicate every operation for every part — but that multiplies the number of
operations in the tree. Instead, run each operation once and use Array to
repeat it at each grid position.

---

## When to use it

- Multiple identical parts in a grid layout on a single stock sheet.
- Repeat drilling patterns across a panel.
- Any scenario where the same toolpath must be executed at multiple locations.

## When NOT to use it

- When different instances need different parameters — create separate operations
  instead.

---

## See also

- [New Job](new-job.md) — job and stock setup
- [CAM Workbench](../index.md) — workbench overview
