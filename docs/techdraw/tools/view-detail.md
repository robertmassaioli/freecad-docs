# Insert Detail View

> **In one sentence:** Creates a magnified inset view of a circular region of
> an existing view — for calling out small features at a larger scale without
> changing the main view scale.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Views → Insert Detail View

A detail view is used when a feature (thread, fillet, tight tolerance) is too
small to dimension clearly at the main view scale. A circle on the parent view
marks the area being magnified, and the detail view appears separately at its
own scale.

---

## Step-by-step

1. Select the parent view on the page.
2. Choose **TechDraw → Views → Insert Detail View**.
3. In the task panel:
   - Click on the parent view to set the centre of the detail circle.
   - Set the circle **Radius** (in page mm, not model mm).
   - Set the **Scale** for the magnified view.
4. Click **OK**.

The detail view appears, labelled with a letter (C, D, …). A corresponding
circle with the same letter appears on the parent view.

---

## When to use it

- Small features (threads, fillets, close tolerances) too small to dimension
  clearly at the main view scale.
- Calling out a complex region without increasing the scale of the whole view.

---

## Common mistakes and pitfalls

!!! warning "Detail view requires a parent view"
    Detail View must be created from an existing parent view. Select the
    parent view before activating this tool.

---

## See also

- [Insert View](view-insert.md) — the parent view required first
- [Insert Section View](view-section.md) — cross-section for internal geometry
- [TechDraw Workbench](../index.md) — workbench overview
