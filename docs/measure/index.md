# Measure Workbench

> **In one sentence:** The Measure workbench provides a unified measurement
> tool that reports distances, angles, lengths, areas, radii, positions,
> and centres of mass from selected geometry — with an optional quick-
> measurement display that shows live results as you hover over objects.

---

## What is the Measure workbench for?

The Measure workbench is FreeCAD's **Unified Measurement Facility (UMF)**
— a single, context-aware tool that detects what you have selected and
applies the most appropriate measurement automatically.

**Common uses:**

- Checking the distance between two faces or vertices before adding
  a gap constraint.
- Reading the angle between two faces on an imported solid.
- Confirming the total edge length of a profile wire.
- Measuring the surface area of a face for coating calculations.
- Finding the radius of a cylindrical hole or arc.
- Locating the centre of mass of a solid for balance analysis.

---

## Measure vs other measurement approaches

| Method | What it shows | Persists in document |
|--------|--------------|---------------------|
| **Measure tool** | Full measurement result with label in 3-D view | Yes (saved as Measure object) |
| **Quick Measure** | Live values in the status bar on selection | No — display only |
| **Part → Check Geometry** | Topological analysis, not measurement | No |
| **TechDraw Dimensions** | Dimensions on 2-D drawing sheet | Yes (in TechDraw page) |
| **Sketcher dimensions** | Driving/reference dimensions inside a sketch | Yes (in sketch) |

Use **Measure** when you want a measurement result displayed in the 3-D
view and saved in the document. Use **Quick Measure** for fast spot checks.

---

## Quick Measure (status bar)

Quick Measure is always active — it requires no activation. As you
**click to select** geometry in the 3-D view, FreeCAD automatically
evaluates what is selected and shows the result in the status bar at
the bottom of the screen:

| Selection | Shown in status bar |
|-----------|-------------------|
| One vertex | X, Y, Z position |
| One edge | Edge length (straight) or arc length |
| One circular edge | Radius |
| One face | Surface area |
| Two vertices | Distance between vertices |
| Two faces | Minimum distance between faces |
| Edge + face | Angle between edge and face |

Quick Measure results are **not** saved — they disappear when the selection
changes. For a persistent annotation, use the full Measure tool.

---

## See also

- [Measure Tool](tools/measure.md) — the unified measurement task panel
