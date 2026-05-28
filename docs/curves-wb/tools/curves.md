# Curve Tools

---

## Parametric Line {#parametric-line}

**Menu:** Curves → Parametric Line  
**Command:** `Curves_line`

Creates a parametric line between two selected vertices. Unlike a Draft Line,
this line updates automatically if either vertex moves (e.g. if it is on a
shape that is modified).

**Usage:** Select 2 vertices in the 3-D view, then activate the tool.

---

## Gordon Profile {#gordon-profile}

**Menu:** Curves → Gordon Profile  
**Command:** `gordon_profile`

Creates an editable profile curve intended for use as an input to the
[Gordon Surface](surfaces.md#gordon-surface) tool. The profile can be
interactively edited with control point handles.

---

## Mixed Curve {#mixed-curve}

**Menu:** Curves → Mixed Curve  
**Command:** `mixed_curve`

Builds a 3-D curve as the **intersection of two projected curves**. Given
two curves projected from different directions (typically plan and elevation
views of a 3-D curve), the intersection defines the 3-D path.

**Usage:** Select two objects or shapes (e.g. a front-view curve and a
side-view curve), then activate the tool.

---

## Extend Curve {#extend-curve}

**Menu:** Curves → Extend Curve  
**Command:** `extend`

Extends an existing edge by a specified distance, maintaining the curve's
tangent direction at the endpoint. The extension can be:
- **Tangential** — straight continuation of the tangent
- **G2 smooth** — curvature-continuous extension

**Usage:** Select an edge in the 3-D view and activate the tool.

---

## Join Curves {#join-curves}

**Menu:** Curves → Join Curves  
**Command:** `join`

Joins multiple selected edges into a single **B-spline curve**. The result is
a continuous spline that passes through the endpoints of all input edges.
Useful for converting a chain of separate edges (e.g. from imported geometry)
into one smooth parametric curve.

**Usage:** Select the edges to join (in order), then activate the tool. Or
select an object containing multiple edges in the tree.

---

## Split Curve {#split-curve}

**Menu:** Curves → Split Curve  
**Command:** `split`

Splits an edge at one or more specified parameter positions, producing
multiple sub-edges. The split points can be:
- At a specified parameter value (0–1)
- At the nearest point to a selected vertex

**Usage:** Select an edge and activate the tool. A task panel lets you add
split points interactively.

---

## Discretize {#discretize}

**Menu:** Curves → Discretize  
**Command:** `Discretize`

Samples an edge or wire into an ordered array of points (a
`Points::Feature`). The sampling can be:

| Mode | Description |
|------|-------------|
| **Number of points** | Fixed count, evenly distributed by parameter |
| **Distance** | Points separated by a specified arc-length distance |
| **Deflection** | Adaptive — more points where curvature is higher |
| **Angular deflection** | Points placed so the chord angle stays below a threshold |
| **Curve length** | Points separated by a specified chord length |

**Usage:** Select an edge in the 3-D view and activate the tool.

---

## Approximate {#approximate}

**Menu:** Curves → Approximate  
**Command:** `Approximate`

Fits a NURBS **curve or surface** to a set of points. The result approximates
the input (does not necessarily pass through every point).

### Parameters

| Parameter | Description |
|-----------|-------------|
| **Degree min / max** | Polynomial degree range |
| **Max Segments** | Maximum number of knot intervals |
| **Continuity** | Internal continuity at knots: C0, C1, C2 |
| **Tolerance** | Fitting tolerance in mm |
| **Parametrization** | Chord length, centripetal, or uniform |

---

## Interpolate {#interpolate}

**Menu:** Curves → Interpolate  
**Command:** `Interpolate`

Creates a NURBS curve that passes **exactly through** each input point (in
contrast to Approximate which allows deviation). Point tangents at the
start and end can be specified.

---

## Blend Curve {#blend-curve}

**Menu:** Curves → Blend Curve  
**Command:** `ParametricBlendCurve`

Creates a **blend curve** — a B-spline that smoothly connects two edge
endpoints with a specified continuity order at each end.

### Continuity options per end

| Continuity | Condition imposed |
|------------|------------------|
| **G1** | Tangent direction matches the source edge |
| **G2** | Curvature (magnitude + direction) matches the source edge |
| **G3** | Rate of curvature change matches the source edge |

The **Size** parameter controls how strongly the blend curve "pulls" toward
each source edge's tangent direction.

Double-click the object to enter freehand mouse-editing mode, where you can
drag the size handles interactively.

---

## Comb Plot {#comb-plot}

**Menu:** Curves → Comb Plot  
**Command:** `ParametricComb`

Displays a **curvature comb** on the selected edges — spines perpendicular
to the curve, with length proportional to curvature. A uniform comb indicates
smooth, even curvature distribution; large spikes indicate curvature peaks.

Use this to diagnose oscillating B-spline control-point distributions before
using a curve as a surface boundary.

---

## Curve on Surface {#curve-on-surface}

**Menu:** Curves → Curve on Surface  
**Command:** `cos`

Creates a parametric curve constrained to lie on a face. The curve is defined
in the face's UV parameter space and projects onto the 3-D surface. If the
surface changes, the curve updates.

Use this for:
- Defining a trim boundary on a face
- Creating a guide rail for a surface-following sweep

---

## Common mistakes and pitfalls

!!! warning "Join Curves produces a coarse approximation"
    **Cause:** The joining algorithm approximates to a B-spline which may
    have fewer degrees of freedom than the input chain.  
    **Fix:** Increase the **Max Degree** or **Max Segments** settings if
    available, or try the Approximate tool with a tighter tolerance.

!!! warning "Blend Curve gaps visible at endpoints"
    **Cause:** The continuity order requested exceeds what the source curve
    can supply (e.g. requesting G2 from a G1 source).  
    **Fix:** Reduce the continuity order, or ensure the source edges have
    sufficient continuity at their endpoint.
