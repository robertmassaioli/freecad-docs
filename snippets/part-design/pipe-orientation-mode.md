### Orientation mode in depth

The Orientation mode dropdown controls how the profile *rotates about the spine
axis* as it travels along the path. The spine tells the profile *where to go*;
the orientation mode tells it *which way to face* at each point along the journey.

#### Standard (default)

FreeCAD minimises the total rotation of the profile as it travels the spine. The
algorithm uses the Darboux frame with minimum-rotation transport. Think of sliding
a coin along a curved rail while keeping the coin's face as stable as possible —
it tilts to follow the rail's slope but does not spin unnecessarily.

**Use this mode when:**
- You want the most natural result with no explicit configuration
- The spine is a flat 2-D curve (Standard and Frenet give identical results for
  planar spines)
- No twist or roll is required in the swept solid

**Watch out for:** On highly curved 3-D spines, Standard can still accumulate a
small twist. If you see an unexpected rotation in the solid, try Frenet or
Auxiliary to diagnose whether the path curvature is the cause.

#### Fixed

The profile keeps its original world-space orientation throughout the entire
sweep. It does not tilt, rotate, or twist at any point — like pushing a square
stamp along a curved groove while your hand stays completely level: the face of
the stamp is always parallel to its starting plane.

**Use this mode when:**
- The profile must maintain an absolute world orientation regardless of the path
  (e.g. a sign bracket that must always face the same compass direction)
- The spine is a 3-D curve but the cross-section must not rotate in world space
- You intentionally want the ends of the sweep *not* to be perpendicular to the
  path

**Watch out for:** On curved spines, a Fixed-orientation profile quickly produces
twisted, self-intersecting geometry because the profile face is no longer tangent
to the path. Fixed is practical only when the path has minimal curvature or the
profile is very small relative to the bend radius.

#### Frenet

Uses the classical Frenet–Serret frame from differential geometry. The profile's
normal tracks the path's curvature vector (the principal normal). Think of a
roller-coaster car whose "up" direction always points toward the centre of the
current curve.

**Use this mode when:**
- You want the profile normal to follow the curvature rather than minimise
  overall rotation
- Working with a mathematically defined curve and Frenet-frame behaviour is
  specifically required
- Standard produces unexpected results and you want a different algorithm to
  compare against

**Watch out for:** On nearly-straight sections of the path (very low curvature),
the Frenet normal is mathematically undefined and can flip 180° — producing a
sudden twist in the solid. This is the classical Frenet "flip" problem. Avoid
Frenet for spines that include straight or near-straight segments.

#### Auxiliary

A second "guide spine" sketch is provided. The profile's roll angle at each
point is controlled by the angular relationship between the primary and auxiliary
spines, giving explicit control over twist. Think of a DNA double helix — you
draw both strands and let FreeCAD work out the rotation.

**Use this mode when:**
- You need a deliberately twisted sweep (propeller blade, twisted square column,
  helical channel)
- Standard and Frenet both produce unwanted rotation and manual control is needed
- The twist must match another piece of geometry (the auxiliary spine encodes it)

Sub-parameters active in Auxiliary mode:

| Sub-parameter | Type | Default | Description |
|---------------|------|---------|-------------|
| Auxiliary spine | Object link + edge list | — | The guide path that drives roll. Should span the full length of the primary spine. |
| Curvilinear equivalence | Checkbox | On | When on, points are distributed by arc length on each spine (natural spacing). When off, parallel projection is used — can produce bunching on unequal-length spines. |

**Watch out for:** Both spines must span the same range — the auxiliary spine
must start and end at the same locations as the primary spine. Mismatched lengths
prevent the orientation mapping from being computed.

#### Binormal

The profile's out-of-plane axis (binormal) is locked to a fixed world-space
vector you specify. Like extruding a shape while always keeping it "pointing
toward north" regardless of how the path curves.

**Use this mode when:**
- The profile must maintain a fixed facing direction throughout (not just stay
  upright, but face a specific world direction)
- Sweeping along a 3-D path where one face of the profile must consistently point
  toward a wall, datum, or reference

Sub-parameters active in Binormal mode:

| Sub-parameter | Type | Default | Description |
|---------------|------|---------|-------------|
| X / Y / Z | Float | 0.0 | The components of the fixed binormal vector. FreeCAD normalises it automatically. All three values being zero is invalid. |

**Watch out for:** A zero binormal vector (0, 0, 0) causes a failure at
recompute. Also, if the path tangent becomes parallel to the binormal vector at
any point along the spine, the orientation is geometrically undefined there and
the sweep fails.
