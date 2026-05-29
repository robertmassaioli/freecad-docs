# Insert Projection Group

> **In one sentence:** Creates a coordinated set of orthographic views
> (front, top, right, isometric) that automatically align on the page —
> the standard way to create a multi-view engineering drawing.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Views → Insert Projection Group

A Projection Group is the recommended way to start a complete engineering
drawing. All views in the group are linked and maintain their relative
alignment — moving the anchor (front) view moves all others automatically.

---

## First-angle vs third-angle projection

| Convention | Standard | Usage |
|------------|----------|-------|
| First angle | ISO / European | Top view below front; right view to the left |
| Third angle | ANSI / US | Top view above front; right view to the right |

Set the convention in the Projection Group's **Projection Type** property
or in **Edit → Preferences → TechDraw → General → Projection Angle**.

---

## Step-by-step

1. Select the 3-D object in the model tree.
2. Choose **TechDraw → Views → Insert Projection Group**.
3. In the task panel:
   - Choose which views to include (Front, Top, Right, Left, Bottom, Back,
     Isometric).
   - Set the scale and anchor view direction.
4. Click **OK**. The views appear aligned on the page.

Moving the anchor view (the front view) moves all other views automatically,
maintaining their alignment.

---

## Common mistakes and pitfalls

!!! warning "Anchor view must be the front view"
    The front view in a Projection Group is the anchor; all other views
    align relative to it. If you change the front view's direction, the
    other views rotate to match. Changing only a side view's direction
    breaks the alignment.

---

## See also

- [Insert View](view-insert.md) — create a single view without a group
- [TechDraw Workbench](../index.md) — workbench overview
