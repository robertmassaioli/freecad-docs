### Orientation mode in depth

The Orientation mode dropdown controls how the profile *rotates about the spine
axis* as it travels along the path. The spine tells the profile *where to go*;
the orientation mode tells it *which way to face* at each point along the journey.

#### Standard (default)

**Intuition:** Like threading a needle onto a curved wire and letting it find its
own least-twisted resting orientation — the profile tilts to follow the path but
never spins more than necessary.

Technically, FreeCAD uses the Darboux frame with minimum-rotation transport
(sometimes called "parallel transport"). The total accumulated twist from start
to end is minimised.

**Use this mode when:**
- You want the most natural result with no explicit configuration
- The spine is a flat 2-D curve (Standard and Frenet give identical results for
  planar paths)
- No deliberate twist or roll is needed in the swept solid

**Watch out for:** On highly curved 3-D spines, Standard can still accumulate a
visible twist. If the finished solid looks rotated at one end, try Frenet or
Auxiliary to diagnose whether the path curvature is causing it.

#### Fixed

**Intuition:** Like carrying a tray of drinks level through a winding corridor —
the profile keeps its original world-space orientation no matter how the path
turns and dips.

The profile's X, Y, Z axes in world space are frozen at their initial values
and never rotate throughout the sweep. The cross-section always faces the same
compass direction.

**Use this mode when:**
- The profile must maintain an absolute world orientation regardless of the path
  (e.g. a sign bracket that must always face front, or a slide channel that must
  stay horizontal)
- You intentionally want the ends of the sweep to *not* be perpendicular to the
  path

**Watch out for:** On curved paths, a profile that cannot tilt to stay tangent
to the path will quickly twist and self-intersect. Fixed is practical only for
paths with minimal curvature, or when the profile is small enough that the
mismatch does not cause self-intersection.

#### Frenet

**Intuition:** Like a roller-coaster car whose "up" arrow always points toward
the inside of the current curve — the profile tips inward on bends, following
the local curvature of the path.

Technically this uses the classical Frenet–Serret frame: the profile normal
follows the principal curvature normal of the path. On a tightly curved section
the profile leans steeply; on a nearly-straight section the lean is almost zero.

**Use this mode when:**
- You want the profile normal to track the path curvature rather than minimise
  overall rotation
- A mathematically defined curve requires Frenet-frame behaviour specifically
- Standard produces unexpected rotation and you need a different algorithm to
  compare

**Watch out for:** On nearly-straight sections (very low curvature), the Frenet
normal is mathematically undefined and can abruptly flip 180° — the classical
Frenet "flip" problem, which produces a sudden twist in the solid. Avoid Frenet
for any spine that includes straight or near-straight segments.

#### Auxiliary

**Intuition:** Like tracing both strands of a twisted rope to define its twist —
you draw a second guide spine alongside the main path, and FreeCAD reads the
angular relationship between them to determine exactly how much the profile
should roll at every point.

This mode gives you the most control: any twist behaviour you can express as a
second curve can be encoded.

**Use this mode when:**
- You need a deliberately twisted sweep (propeller blade, twisted square column,
  helical channel with a specific roll profile)
- Standard and Frenet both produce unwanted rotation and manual control is needed
- The twist must geometrically match another piece of the design

Sub-parameters active in Auxiliary mode:

| Sub-parameter | Type | Default | Description |
|---------------|------|---------|-------------|
| Auxiliary spine | Object link + edge list | — | The guide path that drives roll. Should span the full length of the primary spine. |
| Curvilinear equivalence | Checkbox | On | On: distribute orientation points by arc length (natural, recommended). Off: parallel projection — can produce bunching on spines of unequal length. |

**Watch out for:** The auxiliary spine must span the same range as the primary
spine. Mismatched start/end points prevent the orientation mapping from being
computed and produce an error.

#### Binormal

**Intuition:** Like a satellite dish on a moving arm — no matter which way the
arm points, the dish always faces a fixed direction you specify (north, vertical,
toward a wall).

You provide an X/Y/Z vector; the profile's out-of-plane axis (the binormal) is
locked parallel to that vector throughout the entire sweep.

**Use this mode when:**
- One face of the profile must consistently point toward a fixed world direction
  (not just stay upright, but face a specific direction)
- Sweeping along a 3-D path where a functional face must always point toward a
  mating surface or datum plane

Sub-parameters active in Binormal mode:

| Sub-parameter | Type | Default | Description |
|---------------|------|---------|-------------|
| X / Y / Z | Float | 0.0 | Components of the fixed binormal vector. FreeCAD normalises it; all three being zero is invalid. |

**Watch out for:** A zero binormal vector (0, 0, 0) causes a recompute failure.
Also, if the path tangent becomes exactly parallel to the binormal vector at any
point along the spine, the orientation is geometrically undefined there and the
sweep will fail.
