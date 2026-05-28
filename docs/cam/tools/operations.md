# Machining Operations

Operations define what the machine actually cuts. Each operation is added to
the active Job and appears in the operation list in execution order.

Every operation shares a common set of depth and height parameters, plus
operation-specific geometry and cutting parameters.

---

## Common parameters

### Depths

| Parameter | Description |
|-----------|-------------|
| **Start Depth** | Z level where cutting begins (top of the feature) |
| **Final Depth** | Z level at the bottom of the cut |
| **Step Down** | Maximum depth per pass (the cutter moves down by this amount per Z level) |

### Heights

| Parameter | Description |
|-----------|-------------|
| **Clearance Height** | Z level for rapid moves between cut regions |
| **Safe Height** | Z level for rapid repositioning above the stock |

### Tool controller

Each operation has a **Tool Controller** property that selects which tool
and speed/feed from the Job's tool controller list.

---

## Profile {#profile}

**Menu:** CAM → Profile  
**Command:** `CAM_Profile`

Generates a contour-following toolpath along the edges of selected faces,
following the outside or inside of a 2-D profile.

### Geometry selection

| Selection | Resulting path |
|-----------|----------------|
| One or more **faces** | Profile of each face's perimeter |
| One or more **edges** | Follow the edges directly |
| **No selection** | Profile of the entire model's outer boundary |

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Side** | Which side of the edge to cut: Outside, Inside, Left, Right |
| **Direction** | CW or CCW cutting direction |
| **Offset Extra** | Additional radial offset from the geometry (positive = further away) |
| **Use Compensation** | Apply tool radius compensation (G41/G42) |
| **Handle Multiple Features** | Combine features into one pass, or separate passes |

---

## Pocket Shape {#pocket-shape}

**Menu:** CAM → Pocket Shape  
**Command:** `CAM_Pocket_Shape`

Clears the interior of one or more closed boundaries. The pocket is
cleared in Z-level passes stepping down by **Step Down** until **Final
Depth** is reached.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Pattern** | Clearing pattern: ZigZag, Offset, ZigZagOffset, Line, Grid, Triangle |
| **Angle** | Direction of ZigZag lines (degrees from X) |
| **Step Over Percent** | Tool engagement as a percentage of tool diameter |
| **Use Outline** | If True, add a finishing pass around the pocket wall |
| **Minimum Travel** | Merge passes shorter than this distance |

---

## Face Milling {#face-milling}

**Menu:** CAM → Face Milling  
**Command:** `CAM_MillFace`

Generates a surfacing pass to flatten the top face of the stock. Typically
run first to establish a consistent Z datum.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **BoundBox** | Which faces to surface: selected, job stock, or all |
| **Pattern** | ZigZag, Offset, or Line |
| **Step Over Percent** | Radial step-over between passes |
| **Finish Depth** | Extra depth beyond the face to ensure clean cut |

---

## Helix {#helix}

**Menu:** CAM → Helix  
**Command:** `CAM_Helix`

Generates a helical descending path into a circular hole or feature. The
tool spirals down to the final depth rather than plunging straight.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Direction** | Climb (CW) or conventional (CCW) |
| **Step Over** | Radial distance between helix passes |
| **Keep Tool Down** | If True, do not lift at end of each pass |

---

## Adaptive Clearing {#adaptive-clearing}

**Menu:** CAM → Adaptive  
**Command:** `CAM_Adaptive`

Generates a trochoidal (curvilinear) clearing path that maintains a
**constant tool engagement angle** — also known as adaptive clearing or
high-efficiency milling (HEM). This significantly extends tool life and
allows higher feed rates.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Operation Type** | Clearing (rough) or Profiling (finishing) |
| **Step Over** | Maximum radial engagement (% of tool diameter) |
| **Lift Distance** | How far the tool lifts between passes |
| **Keep Tool Down Ratio** | Ratio at which to keep the tool down vs lift |
| **Force Inside-Out** | Start cuts from the inside of the pocket |

---

## Slot {#slot}

**Menu:** CAM → Slot  
**Command:** `CAM_Slot`

Cuts a slot between two selected points or along an edge.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Layer Mode** | Single pass or multiple Z-level passes |
| **Path Node Offset** | Start/end offsets from the endpoint geometry |

---

## 3D Pocket {#3d-pocket}

**Menu:** CAM → 3D Pocket  
**Command:** `CAM_Pocket3D`

Clears a pocket whose floor is a 3-D curved surface (not horizontal).
The path adapts to the shape of the floor at each Z level.

---

## Surface {#surface}

**Menu:** CAM → Surface  
**Command:** `CAM_Surface`  
**Requires:** OpenCamLib

Generates a 3-D surface machining toolpath across arbitrary curved surfaces.
The path is computed by tracing the surface at a specified step-over distance.

### Scan types

| Type | Description |
|------|-------------|
| **Planar** | Passes parallel to the XY plane (like waterline, but at any angle) |
| **Rotational** | Revolving passes (for turning or 4-axis) |

---

## Waterline {#waterline}

**Menu:** CAM → Waterline  
**Command:** `CAM_Waterline`  
**Requires:** OpenCamLib

Generates horizontal (constant-Z) passes that follow the silhouette of the
model at each Z level. Good for steep walls and undercuts where planar scanning
produces poor surface finish.

---

## Drilling {#drilling}

**Menu:** CAM → Drilling  
**Command:** `CAM_Drilling`

Generates drilling cycles for circular holes. FreeCAD detects circular
edges or cylindrical faces and creates a drill cycle at each centre point.

### Cycle types

| Cycle | G-code | Description |
|-------|--------|-------------|
| **G81** | G81 | Simple drill |
| **G82** | G82 | Drill with dwell |
| **G83** | G83 | Peck drilling (retract between pecks) |
| **G85** | G85 | Boring (feed in, feed out) |

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Drill Mode** | Cycle type (G81, G82, G83…) |
| **Peck Depth** | Depth of each peck in G83 mode |
| **Dwell Time** | Pause at bottom in G82 mode |
| **Extra Offset** | Additional depth beyond the hole bottom |

---

## Engrave {#engrave}

**Menu:** CAM → Engrave  
**Command:** `CAM_Engrave`

Follows selected edges or a `Draft::ShapeString` object at a constant Z
depth. Used for engraving text, logos, and decorative patterns.

---

## Deburr {#deburr}

**Menu:** CAM → Deburr  
**Command:** `CAM_Deburr`

Generates a chamfering pass around the perimeter of a selected face to
remove burrs left by other operations. The tool follows the face boundary
at an offset angle determined by the chamfer angle.

---

## V-Carve {#v-carve}

**Menu:** CAM → V-Carve  
**Command:** `CAM_Vcarve`

Generates a variable-depth carving path using a V-bit. Wider regions are
cut deeper (the V-bit's wedge removes more material), narrower regions
shallower — producing crisp, tapered engraving.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Discretize** | Number of sample points along each edge |
| **Colinear Tolerance** | Threshold for merging colinear path segments |

---

## Comment {#comment}

**Menu:** CAM → Comment  
**Command:** `CAM_Comment`

Inserts a text comment into the G-code output (e.g. `; Begin finish pass`).
Comments have no effect on machine motion.

---

## Stop {#stop}

**Menu:** CAM → Stop  
**Command:** `CAM_Stop`

Inserts a program stop (`M0` — optional stop) or required stop (`M1`) at
the current position in the operation list. The machine stops and waits for
the operator to press Cycle Start.

Use between operations to inspect work-in-progress or change fixturing.

---

## Custom {#custom}

**Menu:** CAM → Custom  
**Command:** `CAM_Custom`

Inserts a block of arbitrary G-code text directly into the output. Use for
machine-specific sub-programmes, user-defined cycles, or G-code that FreeCAD
does not generate natively.

---

## Probe {#probe}

**Menu:** CAM → Probe  
**Command:** `CAM_Probe`

Generates a touch-off probing routine that measures workpiece position and
updates the work coordinate system (WCS). Requires a probe tool and a
machine controller that supports probing cycles (G38.2 etc.).

---

## Common mistakes and pitfalls

!!! warning "Profile cuts on the wrong side of the line"
    **Cause:** The **Side** parameter (Outside vs Inside) is incorrect, or
    the face normal is pointing inward.  
    **Fix:** Check the **Side** parameter. If using edge selection, try
    changing from **Left** to **Right**.

!!! warning "Pocket leaves material in corners"
    **Cause:** The tool diameter is larger than the corner radius. The pocket
    operation cannot reach the corner.  
    **Fix:** Apply a **Dogbone** dress-up to add corner reliefs, or use a
    smaller tool for a finish pass.

!!! warning "Drilling misses some holes"
    **Cause:** FreeCAD only detects holes from circular edges that have a
    full 360° arc. Partial arcs or faces from mesh models are not detected.  
    **Fix:** Use the **Base Geometry** panel to manually add the hole centres,
    or ensure the model has clean circular topology.

!!! warning "Step Down is too large — tool breaks"
    **Cause:** The default Step Down from the Setup Sheet may be too aggressive
    for the material and tool combination.  
    **Fix:** Override the Step Down in the operation's properties. A safe
    starting value is 50 % of the tool diameter for end mills.
