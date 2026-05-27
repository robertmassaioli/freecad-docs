### Mode in depth

The Mode dropdown is the first parameter to set. It determines which three of
the four numeric helix parameters — Pitch, Height, Turns, and Cone angle (or
Radial growth) — are the *primary inputs* you type in, and which one is
*derived automatically*. The greyed-out field in the task panel is the derived
value; changing any primary input recalculates it instantly.

Think of it as choosing which constraints to drive: you always have three
degrees of freedom, the mode picks which three.

#### Pitch-Height-Angle (default)

You enter Pitch and Height; FreeCAD computes Turns. Cone angle is always a
separate, independent input for conical helices.

Intuition: "I know the thread pitch (distance between turns) and the total
length of the feature. Tell me how many turns that gives."

**Use this mode when:**
- Designing standard screws or threads where pitch and length are specified
  (e.g. M6×1.0 thread, 20 mm long — pitch and height are the design inputs)
- Modelling coil springs where the wire pitch and overall spring length are
  the known dimensions
- The turn count is a consequence, not a requirement

**Watch out for:** If Height ÷ Pitch is not an integer, the helix terminates
mid-turn. The solid ends at a partial spiral face, which may be unexpected.
If you need a whole number of turns, switch to Pitch-Turns-Angle instead.

#### Pitch-Turns-Angle

You enter Pitch and Turns; FreeCAD computes Height.

Intuition: "I need exactly 5 full turns with a 1.5 mm pitch. How long is that?"

**Use this mode when:**
- The number of engaging thread turns is the critical design variable (minimum
  thread engagement is typically 1.5 × nominal diameter for steel-into-steel)
- Designing springs where the coil count is specified (number of active coils)
- Fitting a helix into a feature where the turn count, not the length, is fixed

**Watch out for:** The derived Height is Pitch × Turns. If you later change
either primary input and the new Height no longer fits in the available space,
the helix will protrude through other geometry without warning.

#### Height-Turns-Angle

You enter Height and Turns; FreeCAD computes Pitch.

Intuition: "It must fit in a 25 mm pocket and I need at least 8 turns. What
pitch does that require?"

**Use this mode when:**
- The axial space is constrained by the design (e.g. a recess of known depth)
  *and* a minimum number of turns is required (e.g. minimum thread engagement)
- Both height and turn count are specified by a drawing or standard, and pitch
  is the outcome

**Watch out for:** The derived Pitch = Height ÷ Turns. This often yields a
non-standard pitch value — verify it is achievable in manufacturing before
committing. For standard thread pitches, use Pitch-Height-Angle or
Pitch-Turns-Angle instead.

#### Height-Turns-Growth

You enter Height and Turns; FreeCAD computes Pitch. Instead of Cone angle,
the radial shape is controlled by **Radial growth** (the increase in radius per
full turn). This mode is designed for Archimedean spirals, not cylindrical or
conical helices.

Intuition: "I want a flat scroll spring that grows 3 mm outward each turn and
completes 4 turns in a 10 mm axial height." Height ≈ 0 gives a purely flat
spiral.

**Use this mode when:**
- Creating scroll springs, decorative spirals, or cam profiles where the radius
  expands (or contracts) linearly with angle
- Setting Height to 0 to produce a flat planar spiral (zero pitch, expanding
  radius only — requires a small non-zero Tolerance to fuse cleanly)
- The Cone angle concept does not apply and radial growth per turn is the
  natural design input

**Watch out for:** Cone angle is unavailable in this mode (it is replaced by
Radial growth — they are mutually exclusive ways to describe the radial shape).
A negative Radial growth produces an inward-contracting spiral; the radius must
remain positive at all turns or the helix collapses and fails.
