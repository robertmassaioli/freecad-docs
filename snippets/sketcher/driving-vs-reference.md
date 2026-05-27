### Driving vs Reference mode in depth

Every dimensional constraint can operate in one of two modes. Understanding the
difference prevents the most common source of solver conflicts in Sketcher.

#### Driving (default)

A driving constraint **owns the value** — it tells the solver what the measurement
must be, and the solver moves geometry to satisfy it. If you type `25 mm`, the
sketch element is repositioned so that the measurement equals exactly 25 mm.

**Use driving mode when:**

- You are defining the *design intent* of your sketch (the actual sizes you want).
- You want to fully constrain the sketch (green elements, zero degrees of freedom).
- You intend to use the dimension value as a parameter that other features will reference.

**Watch out for:** Adding a driving constraint that conflicts with an existing
constraint (e.g. you already have a coincident + horizontal constraint that fixes
the distance) produces a **redundant constraint** (shown in red). Delete one of
the competing constraints instead of switching to reference mode.

#### Reference (aka Construction/Inspection dimension)

A reference constraint **reads the current measurement** — it does not constrain
geometry. The value shown is computed from whatever the geometry currently is; if
the geometry moves, the value updates. Reference dimensions are shown in blue by
default.

**Use reference mode when:**

- You want to verify a computed dimension without adding a constraint (e.g. check
  the diagonal of a rectangle that is already fully constrained by its width and
  height).
- The constraint would be redundant but you want the value visible for reference.
- You are communicating a derived measurement to a downstream reader of the sketch.

**Watch out for:** Reference constraints do not constrain geometry — if you rely on
them to control a shape, the sketch will remain under-constrained. Changing the
displayed value in the UI has no effect when the constraint is in reference mode.

#### Toggling between modes

- During constraint creation, click the **Reference** radio button in the value
  dialog before confirming.
- After creation, select the constraint and press ++k+x++ (Toggle Driving
  Constraint) to flip it.
- Alternatively right-click the constraint label in the viewport → Toggle driving.
