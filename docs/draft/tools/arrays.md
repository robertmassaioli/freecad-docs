# Array Tools

> **In one sentence:** Array tools create multiple copies of an object in
> structured patterns — rectangular grids, polar rings, concentric circles,
> along a path, or at explicit point locations — with the option to use
> lightweight App::Link instances for large counts.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools

| Tool | Description |
|------|-------------|
| [Ortho Array](#ortho-array) | Rectangular 3-D grid of copies |
| [Polar Array](#polar-array) | Copies arranged in a ring around an axis |
| [Circular Array](#circular-array) | Concentric rings of copies |
| [Path Array](#path-array) | Copies distributed along a curve |
| [Path Link Array](#path-link-array) | Path array using App::Link |
| [Point Array](#point-array) | Copies placed at point object positions |
| [Point Link Array](#point-link-array) | Point array using App::Link |
| [Path Twisted Array](#path-twisted-array) | Path array with twist rotation |
| [Path Twisted Link Array](#path-twisted-link-array) | Twisted path array using App::Link |

---

## Intuition

### Arrays vs App::Link arrays

All array tools have two variants:

- **Standard array** — each element is a full copy of the shape. All
  elements are stored independently in the document. Flexible, supports
  per-element overrides, but heavier on memory.

- **Link array** (Path Link Array, Point Link Array, etc.) — each element
  is an `App::Link` pointing to a single shared shape. All instances share
  one shape object. Much lighter on memory and faster to display for large
  counts (100+ elements).

Use Link arrays for bolt patterns, rivets, and other large uniform grids.
Use standard arrays when you need per-element property overrides.

### Array properties shared by all types

| Property | Description |
|----------|-------------|
| Base | The source object being arrayed |
| Fuse | Fuse all elements into one shape |
| Count / Number | Total number of copies |
| Expand Array | Show individual elements in the model tree |

---

## Ortho Array

**Menu:** Draft → Modification → Array Tools → Ortho Array  
**Command:** `Draft_OrthoArray`

Creates a 3-D rectangular grid of copies along up to three orthogonal
axes (X, Y, Z). Each axis has an independent count and spacing.

### Parameters

| Property | Description |
|----------|-------------|
| Number X | Count along the X axis (includes original) |
| Number Y | Count along the Y axis |
| Number Z | Count along the Z axis |
| Interval X | Spacing vector in X direction |
| Interval Y | Spacing vector in Y direction |
| Interval Z | Spacing vector in Z direction |

**Total elements** = Number X × Number Y × Number Z.

The **Interval** is the centre-to-centre distance between copies, not
the gap. For a 50 mm part with 5 mm clearance, set Interval = 55 mm.

### Example: 3×3 bolt grid

```python
import Draft

bolt = App.ActiveDocument.getObject("Body")
arr = Draft.make_ortho_array(
    obj=bolt,
    v_x=App.Vector(30, 0, 0),
    v_y=App.Vector(0, 30, 0),
    v_z=App.Vector(0, 0, 0),
    n_x=3, n_y=3, n_z=1,
    use_link=True
)
App.ActiveDocument.recompute()
```

---

## Polar Array

**Menu:** Draft → Modification → Array Tools → Polar Array  
**Command:** `Draft_PolarArray`

Creates copies arranged evenly in a ring around a centre point and axis.

### Parameters

| Property | Description |
|----------|-------------|
| Number Polar | Number of copies (including original) |
| Polar Angle | Total arc swept (360° for a full ring) |
| Center | Centre of rotation |
| Axis | Rotation axis (default: Z, perpendicular to working plane) |

For a full ring with N elements equally spaced at 360°/N, set
Polar Angle = 360 and Number Polar = N.

### Example: 6 bolts on a bolt circle

```python
import Draft

bolt = App.ActiveDocument.getObject("Body")
arr = Draft.make_polar_array(
    obj=bolt,
    number=6,
    angle=360,
    center=App.Vector(0, 0, 0),
    use_link=True
)
App.ActiveDocument.recompute()
```

---

## Circular Array

**Menu:** Draft → Modification → Array Tools → Circular Array  
**Command:** `Draft_CircularArray`

Creates concentric rings of copies, like ripples from a stone in water.
The first ring is at a given radius from the centre; subsequent rings
are spaced by a symmetry point distance.

### Parameters

| Property | Description |
|----------|-------------|
| Number Circles | Number of concentric rings |
| Radial Distance | Radial distance between successive rings |
| Symmetry | Number of copies per ring (rotational symmetry) |
| Center | Centre point of the circular pattern |
| Axis | Normal to the pattern plane |

The innermost ring has `Symmetry` copies at `Radial Distance`. The next
ring has more copies at `2 × Radial Distance`, and so on — the count per
ring increases to maintain approximate angular spacing.

---

## Path Array

**Menu:** Draft → Modification → Array Tools → Path Array  
**Command:** `Draft_PathArray`

Distributes copies along a curve (the path). The copies can be oriented
so that one axis of each copy follows the path tangent.

### Parameters

| Property | Description |
|----------|-------------|
| Path Object | The wire, edge, or spline to distribute along |
| Count | Number of copies |
| Extra Translation | Additional offset applied to each copy |
| Align | Rotate each copy to follow the path tangent |
| Align Mode | Original / Frenet / Tangent |
| Tan Vector | Local axis to align with the path tangent |
| Force Vertical | Keep the up axis always vertical |
| Vertical Vector | The axis to keep vertical when Force Vertical is on |
| Start Offset / End Offset | Fraction of path length to leave empty at start/end |

### Align modes

| Mode | Description |
|------|-------------|
| Original | Keep the original orientation of the copy — no alignment |
| Frenet | Align with the Frenet-Serret frame of the path (natural rotation) |
| Tangent | Align a chosen local axis with the path tangent |

### Example: fence posts along a curved path

```python
import Draft

post = App.ActiveDocument.getObject("Body")
path = App.ActiveDocument.getObject("Wire")
arr = Draft.make_path_array(
    obj=post,
    pathobjwire=path,
    count=10,
    use_link=True,
    align=True
)
App.ActiveDocument.recompute()
```

---

## Path Link Array

**Menu:** Draft → Modification → Array Tools → Path Link Array  
**Command:** `Draft_PathLinkArray`

Identical to Path Array but creates `App::Link` instances instead of
full copies. Use this for large path arrays (50+ elements) to reduce
file size and display overhead.

---

## Point Array

**Menu:** Draft → Modification → Array Tools → Point Array  
**Command:** `Draft_PointArray`

Places one copy of the source object at each point in a **point object**.
The point object can be a Draft Wire (vertices used as positions), a
Part Compound of vertices, or a dedicated Points object.

### Workflow

1. Create a Draft Wire marking out the desired positions (each vertex =
   one array element position).
2. Select the object to array.
3. Activate **Draft → Modification → Array Tools → Point Array**.
4. Select the point wire (or compound of points).

### Extra translation and rotation

Each copy is placed at the corresponding point with an optional per-point
rotation taken from a rotation attribute (if the point object stores
rotation data per point).

---

## Point Link Array

**Menu:** Draft → Modification → Array Tools → Point Link Array  
**Command:** `Draft_PointLinkArray`

Identical to Point Array but uses `App::Link` instances. Preferred for
large point arrays.

---

## Path Twisted Array

**Menu:** Draft → Modification → Array Tools → Path Twisted Array  
**Command:** `Draft_PathTwistedArray`

Like Path Array, but adds a **twist rotation** that accumulates along the
path. Each copy is rotated by an additional increment relative to the
previous, producing a helical or twisted pattern.

### Additional parameters

| Property | Description |
|----------|-------------|
| Twist Start | Rotation angle of the first element (degrees) |
| Twist End | Rotation angle of the last element |

Intermediate elements are interpolated linearly between Twist Start and
Twist End.

**Use cases:**
- Twisted column arrangements in architecture.
- Helical blade patterns (though Part Helix + Pipe is more precise for
  structural helices).
- Decorative spiral arrangements.

---

## Path Twisted Link Array

**Menu:** Draft → Modification → Array Tools → Path Twisted Link Array  
**Command:** `Draft_PathTwistedLinkArray`

Link variant of Path Twisted Array.

---

## Common mistakes and pitfalls

!!! warning "Array Interval is centre-to-centre, not gap"
    The Interval X/Y/Z in Ortho Array is the vector from one copy's origin
    to the next copy's origin — not the gap between their surfaces. For a
    20 mm part with 5 mm clearance, set Interval = 25 mm.

!!! warning "Polar Array angle is the total swept angle, not the step"
    Polar Angle = 360° with Number = 6 places 6 elements at 0°, 60°, 120°,
    180°, 240°, 300°. If you set Polar Angle = 60° with Number = 6, you get
    6 elements crammed into 60°, spaced 12° apart. The step = Polar Angle /
    (Number − 1) when Polar Angle < 360°, or Polar Angle / Number for a
    full ring.

!!! warning "Path Array Align requires a clean path tangent"
    If the path has sharp corners (a polyline, not a smooth spline), the
    tangent direction changes discontinuously at each vertex. Copies near
    corners may flip or rotate unexpectedly. Use a smooth B-spline path for
    well-behaved aligned arrays.

!!! warning "Link arrays cannot be exploded directly"
    An App::Link array shows all copies but they share one shape. You cannot
    select and modify one element independently. If you need per-element
    overrides, use a standard (non-Link) array, or convert to individual
    objects using the Expand Array option.

---

## See also

- [Modification Tools](modification.md) — Clone, Move, Rotate as simpler single-copy tools
- [Drafting Tools](drafting.md) — creating the base object to array
