# Analysis and Utilities

---

## Zebra Tool {#zebra-tool}

**Menu:** Surfaces → Zebra Tool  
**Command:** `ZebraTool`

Displays a **zebra stripe** pattern on selected faces — alternating black and
white stripes rendered as if reflected from an infinite striped light source.
This is a standard technique for visually detecting surface continuity:

| Pattern at junction | Continuity level |
|--------------------|------------------|
| Stripes align but are broken (step at junction) | G0 (position only) |
| Stripes align and are continuous | G1 (tangent) |
| Stripes align, continuous, and kink-free | G2 (curvature) |

A rough surface finish or oscillating B-spline distribution will show as
irregular stripe spacing even away from junctions.

### Parameters

| Parameter | Description |
|-----------|-------------|
| **Stripe width** | Controls the width of each stripe |
| **Rotation** | Rotates the stripe pattern orientation |

---

## Reflect Lines {#reflect-lines}

**Menu:** Surfaces → Reflect Lines  
**Command:** `ReflectLines`

Highlights **reflection lines** — the curves on a surface where a parallel
light source (defined by a view direction) would be specularly reflected
toward the viewer. Reflection lines are more sensitive to surface quality
than zebra stripes and reveal subtle G2 discontinuities.

**Usage:** Select a face or shell, then activate the tool. The reflection
lines are computed for the current view direction and update when the view
rotates.

---

## Surface Analysis {#surface-analysis}

**Menu:** Surfaces → Surface Analysis  
**Command:** `Curves_SurfaceAnalysis`

Displays a **false-colour map** of surface curvature over a face or shell.
Different curvature types can be mapped:

| Mode | Description |
|------|-------------|
| **Gaussian curvature** | Product of principal curvatures; zero = developable |
| **Mean curvature** | Average of principal curvatures |
| **Min curvature** | Smaller principal curvature |
| **Max curvature** | Larger principal curvature |

The colour scale runs from blue (low / negative) through green to red (high /
positive). A uniform colour indicates even curvature distribution.

---

## Draft Analysis {#draft-analysis}

**Menu:** Surfaces → Draft Analysis  
**Command:** `Curves_DraftAnalysis`

Displays a false-colour map of **draft angles** — the angle between each face
normal and the specified mould-release direction (typically +Z). This is used
to check whether a part can be demoulded without undercuts.

| Colour | Meaning |
|--------|---------|
| **Green** | Sufficient positive draft (face releases cleanly) |
| **Yellow** | Near-zero draft (marginal) |
| **Red** | Negative draft (undercut — part will not release) |

**Usage:** Select a shape, then activate the tool. Set the **pull direction**
in the task panel.

---

## Geometry Info {#geometry-info}

**Menu:** Misc. → Geometry Info  
**Command:** `GeomInfo`

Displays geometric properties of the selected shape in a dockable panel:

- **Edge** — length, curve type (line/circle/B-spline/…), degree, continuity
- **Face** — area, surface type, UV bounds
- **Vertex** — XYZ coordinates
- **Wire** — total length, closed/open status
- **Shell / Solid** — volume, surface area, bounding box

Useful for quickly verifying dimensions or inspecting the topology of imported
geometry without switching to the Measure workbench.

---

## Adjacent Faces {#adjacent-faces}

**Menu:** Misc. → Adjacent Faces  
**Command:** `Curves_adjacent_faces`

Highlights all faces that share an edge with the currently selected face.
The selected face is highlighted in one colour; its neighbours in another.

Use this to:
- Understand the topology of a complex shell
- Find faces that need a blend surface between them
- Identify disconnected shells (faces with no neighbours)

---

## Extract Shapes {#extract-shapes}

**Menu:** Misc. → Extract Shapes  
**Command:** `extract`

Extracts individual sub-shapes from a compound or solid and creates
independent shape objects in the model tree. This allows you to:

- Isolate a single face from an imported solid for further surface work
- Work with individual edges from a compound wire
- Separate shells from a multi-shell solid

**Usage:** Select the compound or solid and activate the tool. All sub-shapes
are extracted simultaneously.

---

## Parametric Solid {#parametric-solid}

**Menu:** Misc. → Parametric Solid  
**Command:** `solid`

Creates a closed solid from a set of selected surfaces (shells). The selected
faces must form a closed, watertight shell. The result is a `Part::Feature`
solid that updates if the source surfaces change.

This is the CurvesWB equivalent of Part's **Convert to Solid**, but
parametric — the solid rebuilds if the input shells are modified.

---

## To Console {#to-console}

**Menu:** Misc. → To Console  
**Command:** `to_console`

Prints the selected shape's properties to FreeCAD's **Python console** as a
Python script fragment. The output includes:

- Shape type and topology summary
- Key geometric properties
- For edges: the underlying curve object with its parameters

Use this when you need to inspect or reproduce a shape's geometry
programmatically, or when diagnosing unexpected shape behaviour.

---

## B-Spline to Console {#bspline-to-console}

**Menu:** Misc. → B-Spline to Console  
**Command:** `Curves_bspline_to_console`

Exports the full definition of a selected B-spline edge to the Python console,
including:

- **Control points** — XYZ coordinates of every pole
- **Knot vector** — the full non-uniform knot sequence
- **Weights** — rational weights (for NURBS)
- **Degree**

The output is formatted as Python code that can reconstruct the curve:

```python
import Part
bs = Part.BSplineCurve()
bs.buildFromPolesMultsKnots(
    poles=[...],
    mults=[...],
    knots=[...],
    periodic=False,
    degree=3,
    weights=[...],
    CheckRational=True
)
```

Use this to:
- Archive a manually designed B-spline for reuse in scripts
- Debug a curve that behaves unexpectedly by examining its knot structure
- Transfer control point data to an external CAD or analysis application

---

## Common mistakes and pitfalls

!!! warning "Zebra stripes flicker when rotating the view"
    **Cause:** The stripe pattern is view-dependent — it is computed for the
    current camera angle.  
    **Fix:** This is expected behaviour. Pause rotation to read the stripe
    pattern, or switch to a fixed orthographic view.

!!! warning "Surface Analysis shows uniform colour on a visibly curved surface"
    **Cause:** The colour scale auto-ranges to the data — if curvature
    variation is small, even a curved surface can appear nearly uniform.  
    **Fix:** Inspect the legend values. The surface may be nearly developable
    (low Gaussian curvature) even if it is visually curved.

!!! warning "Extract Shapes creates too many objects"
    **Cause:** A solid or compound with many sub-faces extracts each face
    individually.  
    **Fix:** Select only the sub-shape you need using the 3-D view's face
    selection mode (hover + click on the face), then extract.
