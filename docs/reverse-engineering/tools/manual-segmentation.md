# Manual Segmentation

> **In one sentence:** Paint mesh faces interactively with a brush to define
> user-labelled regions when automatic segmentation is insufficient.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Segmentation → Manual Segmentation…  
**Shortcut:** none

Opens a task panel with a brush tool and a segment list. You select a segment
label and paint faces onto it by dragging the mouse over the mesh in the 3-D
view. Each segment becomes a separate `Mesh::Feature` in a "Segments" group.

---

## Intuition

Think of it like painting by numbers, but you decide where each colour goes.
You have a blank mesh and a list of labelled buckets (segments). You pick a
bucket and drag your brush over the faces you want to assign to it. When you
are done, each painted region becomes its own sub-mesh.

Use this when the automatic [Mesh Segmentation](mesh-segmentation.md) gets
the boundaries wrong — for example where two surface types blend smoothly into
each other and the curvature threshold is ambiguous.

---

## When to use it

- Automatic Mesh Segmentation produces wrong segment boundaries and you need
  to correct them.
- The mesh has ambiguous curvature transitions that no threshold setting can
  separate correctly.
- You have domain knowledge about the part geometry and can paint the correct
  boundaries by hand.

---

## When NOT to use it

- **Large meshes with clear geometric primitives** — use
  [Mesh Segmentation](mesh-segmentation.md) for automatic classification; it
  is faster and more consistent.
- **Already-split meshes** — use [From Components](from-components.md).

---

## Step-by-step

1. Choose **Reverse Engineering → Segmentation → Manual Segmentation…**.
2. In the task panel:
   - Select the mesh from the **Shapes** drop-down.
   - Click **Add** under the Segments list to create a new segment label.
3. Select the segment you want to paint into.
4. Left-click and drag over mesh faces in the 3-D view to paint them onto the
   selected segment.
5. Right-click and drag to remove faces from the current segment.
6. Scroll the mouse wheel to change the brush size.
7. Repeat steps 3–6 for each region.
8. Click **OK**. FreeCAD creates a "Segments" group with one `Mesh::Feature`
   per painted segment.

---

## Parameters (brush controls)

| Control | Action |
|---------|--------|
| Left-click drag | Add faces to the current segment |
| Right-click drag | Remove faces from the current segment |
| Scroll wheel | Increase / decrease brush radius |

---

## Common mistakes and pitfalls

!!! warning "Painted faces belong to the wrong segment"
    **Cause:** The segment selection in the list did not change before painting.  
    **Fix:** Click the correct segment in the Segments list before painting.
    The active segment should be highlighted.

!!! warning "Brush paints through the mesh to the far side"
    **Cause:** The brush applies to all faces projected under the cursor,
    including faces on the back of the mesh.  
    **Fix:** Rotate to a view where only the intended faces are facing the
    camera, or use the zoom to restrict the field of view.

---

## Python API

Manual Segmentation is an interactive-only tool. Access the results after
creation:

```python
import FreeCAD as App

doc = App.ActiveDocument
segs_group = doc.getObject("Segments")   # or "Segments MyMesh"
if segs_group:
    for obj in segs_group.Group:
        print(obj.Name, "—", obj.Mesh.CountFacets, "faces")
```

---

## See also

- [Mesh Segmentation](mesh-segmentation.md) — automatic curvature-based segmentation
- [From Components](from-components.md) — one segment per disconnected component
- [Approximate Plane](approx-plane.md) — fit a plane to a segment
- [Reverse Engineering Workbench](../index.md) — workbench overview
