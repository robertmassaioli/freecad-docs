### Corner transition in depth

The Corner transition dropdown controls what happens when the spine has sharp
kinks — points where two edges meet at an angle rather than blending smoothly.
The profile must somehow "navigate" each corner.

#### Transformed (default)

At each kink, the profile is sheared to match the local tangent of the incoming
edge. The transition is abrupt — there is no blending radius.

**Use this mode when:**
- The spine has gentle curves where shearing does not cause self-intersection
- You want the mathematically simplest result with no added geometry at corners
- The spine segments are tangent-continuous (no actual kinks)

**Watch out for:** At tight bends or very large profiles, the sheared section
can self-intersect. FreeCAD will report "Pipe could not be built". Reduce the
profile size, increase the bend radius in the spine, or switch to Round corner.

#### Right corner

The sweep makes a sharp mitre cut at each corner — like picture-frame moulding,
where two pieces meet at a 45° mitre rather than bending around.

**Use this mode when:**
- You need a crisp architectural corner with no rounding (square conduit,
  sheet-metal box sections, rectangular hollow sections)
- The profile is rectangular and a mitred join is geometrically appropriate

**Watch out for:** Produces flat triangular corner faces. At very acute corner
angles the mitre face becomes large and the resulting topology is complex, which
can cause downstream Boolean or fillet operations to fail.

#### Round corner

The corner is smoothed with a fillet arc. The pipe curves gradually around the
kink rather than making a sharp turn.

**Use this mode when:**
- The profile is round (pipe, rod, wire) and a smooth bend is physically expected
- The spine has tight corners where Transformed produces self-intersection
- Modelling real-world tubing, handrails, conduit runs, or bent bar stock

**Watch out for:** The fillet radius is computed automatically by OCCT from the
geometry — you cannot specify it directly. The only way to control the radius is
to change the corner angle in the spine sketch or add intermediate tangent points
to guide the curvature.
