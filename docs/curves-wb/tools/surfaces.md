# Surface Tools

---

## Trim Face {#trim-face}

**Menu:** Surfaces → Trim Face  
**Command:** `Trim`

Trims a face using a projected curve. The curve is projected onto the face
along a specified direction, and the portion of the face on the selected side
of the projection is removed.

**Usage:**

1. Select a face to trim.
2. Select the cutter curve (edge or wire).
3. Activate the tool.
4. In the task panel, choose the projection direction and which side to keep.

---

## IsoCurve {#isocurve}

**Menu:** Surfaces → IsoCurve  
**Command:** `IsoCurve`

Extracts iso-parameter curves from a face — lines of constant U or constant V
in the face's parametric space. These are useful as:

- Guide rails for a sweep
- Visual aids for inspecting surface quality
- Input curves for a Gordon surface

### Parameters

| Parameter | Description |
|-----------|-------------|
| **Direction** | U or V iso-curves |
| **Number of curves** | How many iso-curves to create |
| **Parameter value** | Single specific parameter to extract one iso-curve |

---

## Sketch on Surface {#sketch-on-surface}

**Menu:** Surfaces → Sketch on Surface  
**Command:** `SoS`

Maps a flat sketch onto a curved surface. The sketch is wrapped from the XY
plane onto the target face, following the face's UV parametric mapping.

This is useful for:
- Engraving lettering or logos onto a curved part
- Projecting a 2D pattern onto a free-form surface
- Creating trim curves that follow a surface's shape

### Behaviour

The sketch's XY extent is mapped to the face's full UV range (0–1 in both
directions). Scale the sketch to control which portion of the face is covered.
The result updates if the underlying face changes.

!!! warning "Sketch must lie flat in XY plane"
    Any Z offset in the sketch will be ignored. All geometry must be at Z = 0.

---

## Map on Face {#map-on-face}

**Menu:** Surfaces → Map on Face  
**Command:** `Curves_MapOnFace`

Maps a set of objects (edges, wires, or shapes) onto a target face. Unlike
Sketch on Surface which works exclusively with Sketcher sketches, Map on Face
accepts any edge or wire object.

**Usage:** Select the objects to map, then Ctrl-click the target face, then
activate the tool.

---

## Sweep 2 Rails {#sweep-2-rails}

**Menu:** Surfaces → Sweep 2 Rails  
**Command:** `sw2r`

Sweeps a profile curve along two guide rails, producing a surface whose
edges lie on the rails. The profile shape changes along the sweep to connect
the two rails at each point.

### Selection order

1. First rail (edge or wire)
2. Second rail (edge or wire)
3. Profile (edge or wire) — connects the two rails at the start

The rails and profile must share endpoints (the profile endpoints must touch
both rails at the same U parameter).

### Parameters

| Parameter | Description |
|-----------|-------------|
| **Continuity** | Continuity at the start / end profile: G0, G1, G2 |
| **Scale** | Scale factor for the profile shape along the sweep |

---

## Pipeshell {#pipeshell}

**Menu:** Surfaces → Pipeshell  
**Command:** `pipeshell`

Creates a pipe-like surface by sweeping one or more profiles along a spine
curve with explicit orientation control.

Pipeshell gives more control over orientation than a standard BRep `makePipe`:

| Mode | Description |
|------|-------------|
| **Fixed** | Profile maintains a fixed orientation in space |
| **Frenet** | Profile follows the curve's Frenet frame (may twist) |
| **Corrected Frenet** | Frenet with twist minimisation |
| **Binormal** | Profile normal tracks the curve's binormal |
| **Tangential** | Profile tangent aligned to rail tangent |
| **Auxiliary** | Orientation guided by a secondary rail or object |

---

## Gordon Surface {#gordon-surface}

**Menu:** Surfaces → Gordon Surface  
**Command:** `gordon`

Creates a surface that **exactly interpolates a network of crossing curves**.
The surface passes through every curve in the network (both U-direction and
V-direction families).

### Input requirements

- At least 2 U-direction curves and 2 V-direction curves
- Each U curve must intersect each V curve exactly once
- The intersection points must be geometric (within tolerance)
- Use [Gordon Profile](curves.md#gordon-profile) curves for best results

### Recommended workflow

1. Create U-family curves (cross-sections) using Gordon Profile.
2. Create V-family curves (longitudinal guides) using Gordon Profile.
3. Select all U curves, then all V curves.
4. Activate Gordon Surface.

!!! warning "Intersections must be within tolerance"
    If the curves do not physically intersect, the Gordon Surface tool fails.
    Use **Extend Curve** or adjust control points so curves cross exactly.

---

## Segment Surface {#segment-surface}

**Menu:** Surfaces → Segment Surface  
**Command:** `segment_surface`

Divides a surface (shell or solid face) into segments along iso-parameter
lines at specified U and V values. The result is a compound of sub-faces.

**Usage:** Select a face, activate the tool. In the task panel, add U or V
split parameters (0–1). Each split produces an additional sub-face.

---

## Multi-Loft {#multi-loft}

**Menu:** Surfaces → Multi-Loft  
**Command:** `MultiLoft`

Creates a loft surface through a sequence of profiles, where each profile
may itself be composed of **multiple faces** rather than a single wire.

This extends the standard Part Loft by accepting profile objects made of
faces (compound solids or shells) rather than just wires or edges, making it
suitable for lofting between cross-sections with interior structure.

---

## Blend Surface {#blend-surface}

**Menu:** Surfaces → Blend Surface  
**Command:** `Curves_BlendSurf2`

Creates a surface that smoothly connects two edges with a controlled
continuity at each edge. The blend surface fills the gap between two adjacent
faces, matching tangency or curvature at each boundary.

### Continuity options

| Continuity | Condition imposed |
|------------|------------------|
| **G0** | Surface passes through the edge (position only) |
| **G1** | Surface is tangent to the adjacent face at the edge |
| **G2** | Surface matches the curvature of the adjacent face at the edge |

**Usage:** Select the two boundary edges (with their owner faces to define
continuity reference), then activate the tool.

---

## Blend Solid {#blend-solid}

**Menu:** Surfaces → Blend Solid  
**Command:** `Curves_BlendSolid`

Creates a solid blend (fillet-like transition solid) between two faces.
Similar to Blend Surface but the result is a solid body rather than a surface,
making it suitable for adding material between two solid faces.

---

## Flatten Face {#flatten-face}

**Menu:** Surfaces → Flatten Face  
**Command:** `Curves_FlattenFace`

Unrolls a **conical or cylindrical** face into a flat 2-D sheet. The
unrolled shape has the same arc length along every profile curve as the
original 3-D face.

Use this to:
- Generate flat patterns for sheet metal bent around a cylinder
- Lay out fabric or veneer patterns that will be wrapped onto a surface

!!! warning "Only works on developable surfaces"
    Flatten Face requires a surface with zero Gaussian curvature
    (cone, cylinder, or their trimmed subsets). Attempting to flatten a
    doubly-curved surface (sphere, free-form NURBS) will produce distorted
    or incorrect results.

---

## Rotation Sweep {#rotation-sweep}

**Menu:** Surfaces → Rotation Sweep  
**Command:** `Curves_RotationSweep`

Sweeps one or more profiles around a **centre point** (rather than along a
path), creating a surface by rotating the profile about the sweep centre.

Unlike Part Revolve which rotates around a fixed axis, Rotation Sweep allows
the centre point to be anywhere, producing surfaces like turbine blades or
propeller forms.

---

## Truncate/Extend {#truncate-extend}

**Menu:** Surfaces → Truncate/Extend  
**Command:** `Curve_TruncateExtendCmd`

Cuts a shape at a specified plane and either:
- **Truncates** — removes material beyond the plane
- **Extends** — extrudes the cut face to a new position

The cutting plane is defined by selecting a face or datum plane.

---

## Waterline Curves {#waterline-curves}

**Menu:** Surfaces → Waterline Curves  
**Command:** `Curves_WaterlineCurves`

Generates horizontal cross-section contours (waterlines) on a surface or
solid at a series of Z heights. The result is a set of edges tracing the
intersection of horizontal planes with the shape.

### Parameters

| Parameter | Description |
|-----------|-------------|
| **Number of levels** | How many horizontal cross-sections to generate |
| **Z min / Z max** | Height range |
| **Step** | Spacing between levels (alternative to count) |

Waterline curves are useful for:
- Visualising 3-D hull or terrain shapes as 2-D contours
- Generating level curves for milling toolpaths
- Checking a shape's cross-sectional profile

---

## Common mistakes and pitfalls

!!! warning "Gordon Surface fails with 'curves do not intersect'"
    **Cause:** The U and V family curves do not actually cross within the
    geometric tolerance.  
    **Fix:** Use Extend Curve to ensure curves overlap, or check for gaps
    by zooming in to each intended intersection point.

!!! warning "Sketch on Surface produces distorted result"
    **Cause:** The target face has a non-uniform UV parametrisation, so equal
    XY distances in the sketch map to unequal arc lengths on the surface.  
    **Fix:** Accept the distortion (it is inherent to UV mapping) or use
    Curve on Surface to manually place curves by tracing the face's geometry.

!!! warning "Flatten Face gives wrong shape"
    **Cause:** The face is not a pure cylinder or cone (it may be a NURBS
    surface that approximates one).  
    **Fix:** Rebuild the source face using a proper cylindrical or conical
    construction before flattening.
