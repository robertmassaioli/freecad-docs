# Dress-up Operations

Dress-ups are **path modifiers** applied on top of an existing operation.
They do not generate new cutting moves from scratch — instead they transform,
extend, or restrict the path produced by the base operation.

To apply a dress-up:

1. Select the operation you want to modify in the Job's operation list.
2. Go to **CAM → Dress-up** and choose the desired dress-up.
3. A new dress-up object is created on top of the selected operation.

Multiple dress-ups can be stacked — each applies on the output of the
previous one.

---

## Dogbone {#dogbone}

**Menu:** CAM → Dress-up → Dogbone  
**Command:** `CAM_DressupDogbone`

Adds **dogbone** (or T-bone) corner reliefs at inside corners of a pocket or
profile path. A full-radius end mill cannot cut a true right-angle inside
corner — it leaves a radius of material. A dogbone relief over-cuts a small
circle at each corner so that a square-cornered part (e.g. a dovetail or
tenon) can fit.

### Dogbone styles

| Style | Description |
|-------|-------------|
| **Dogbone** | A circular over-cut tangent to both wall faces |
| **T-bone** | A T-shaped slot along one wall face |
| **Horizontal** | Extension perpendicular to the X-axis |
| **Vertical** | Extension perpendicular to the Y-axis |

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Incision** | Percentage of tool diameter for the relief cut |
| **Custom** | Fixed extra distance instead of percentage |
| **Side** | Which side gets the relief (left, right, or both) |

---

## Holding Tabs {#holding-tabs}

**Menu:** CAM → Dress-up → Tag  
**Command:** `CAM_DressupTag`

Adds **holding tabs** — small bridges of uncut material left in a profile
path to keep the part from shifting or flying out during the final cut.
After machining, tabs are cut through manually.

### Adding tabs

1. Apply the Tag dress-up to a Profile operation.
2. In the task panel, click locations on the path where you want tabs.
3. Set tab Width, Height, and Angle (angled tabs are stronger).

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Width** | Tab width along the path |
| **Height** | Tab height (how much material is left) |
| **Angle** | Tab face angle (90° = vertical wall, <90° = chamfered) |

---

## Lead In/Out {#lead-inout}

**Menu:** CAM → Dress-up → Lead In/Out  
**Command:** `CAM_DressupLeadInOut`

Adds a **lead-in arc** at the start of a profile pass and a **lead-out arc**
at the end. The arcs approach and depart the profile tangentially, avoiding
the dwell marks and over-cut that would result from plunging straight into
the cut.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Lead In Length** | Length of the lead-in arc (mm) |
| **Lead Out Length** | Length of the lead-out arc (mm) |
| **Lead In Arc** | Angle of the arc (degrees) |
| **Lead Out Arc** | Angle of the exit arc |
| **Start Radius** | Radius of lead-in arc |
| **End Radius** | Radius of lead-out arc |
| **Use Lead in** / **Use Lead out** | Enable/disable each individually |

---

## Ramp Entry {#ramp-entry}

**Menu:** CAM → Dress-up → Ramp Entry  
**Command:** `CAM_DressupRampEntry`

Replaces straight plunge entries (straight down Z) with a **ramping** or
**helical** entry. Plunging straight down is hard on end mills; ramping
distributes the load over the approach distance.

### Ramp methods

| Method | Description |
|--------|-------------|
| **Ramp** | Linear ZigZag ramp descending to the start depth |
| **Helix** | Helical spiral descending to the start depth |
| **Plunge** | Keep straight plunge (no change — bypass mode) |

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Ramp Method** | Ramp, Helix, or Plunge |
| **Angle** | Ramp angle from horizontal (degrees). Smaller = gentler. |
| **Ramp Feed Rate** | Override feed rate during the ramp |

---

## Boundary {#boundary}

**Menu:** CAM → Dress-up → Path Boundary  
**Command:** `CAM_DressupPathBoundary`

Clips the operation's path to a user-specified boundary shape. Path segments
outside the boundary are removed; path segments partially inside are trimmed.

Use this to restrict an operation (e.g. a large adaptive clearing) to a
specific region without creating a new face selection.

### Boundary sources

| Source | Description |
|--------|-------------|
| **Selection** | Use the selected shape as the boundary |
| **Model** | Use the job model's bounding box |
| **Stock** | Use the stock shape |

---

## Axis Map {#axis-map}

**Menu:** CAM → Dress-up → Axis Map  
**Command:** `CAM_DressupAxisMap`

Remaps one motion axis to another. For example, maps Z-axis motion to
Y-axis motion for a machine with a horizontal spindle, or maps X to A for
a 4-axis rotary setup.

---

## Z Correct {#z-correct}

**Menu:** CAM → Dress-up → Z Correct  
**Command:** `CAM_DressupZCorrect`

Applies a **Z-height correction map** read from a probing file. After
probing the workpiece surface, this dress-up adds the measured height
offsets to every Z move in the path, compensating for workpiece warp or
table non-flatness.

---

## Array (dress-up) {#array}

**Menu:** CAM → Dress-up → Array  
**Command:** `CAM_DressupArray`

Repeats the selected operation in a rectangular or circular array. This is
the dress-up equivalent of the `CAM_Array` modification tool — it applies
array repetition as a post-processing step rather than duplicating operations.

---

## Drag Knife {#drag-knife}

**Menu:** CAM → Dress-up → Drag Knife  
**Command:** `CAM_DressupDragKnife`

Adapts a path for a **drag knife** cutter — a blade that trails the motion
direction. At each sharp corner, the path is extended with a small "swivel"
arc to allow the blade to rotate to the new direction before the corner.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| **Angle** | Minimum corner angle that triggers a swivel (degrees) |
| **Radius** | Swivel arc radius |

---

## Common mistakes and pitfalls

!!! warning "Dogbone dress-up applied to a Pocket — corners still not clean"
    **Cause:** Dogbone dress-up works on the path, not the geometry. For a
    Pocket operation, the over-cuts must be enabled in the Pocket's own
    corner handling, or applied to a Profile finish pass.  
    **Fix:** Apply a Profile finish pass around the pocket perimeter and
    add the Dogbone dress-up to that Profile.

!!! warning "Lead-in arc cuts outside the stock"
    **Cause:** The lead-in arc starts outside the stock boundary.  
    **Fix:** Reduce Lead In Length, or apply a Boundary dress-up to clip
    the lead arc to the stock.

!!! warning "Ramp Entry produces incorrect depth"
    **Cause:** The ramp angle is too steep for the operation's step-down,
    causing the ramp to overshoot the target depth.  
    **Fix:** Reduce the ramp angle or reduce the Step Down.
