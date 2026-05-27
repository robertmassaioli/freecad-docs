### Transform mode in depth

The Transform mode dropdown controls whether the profile shape stays constant
or morphs along the path.

#### Constant (default)

The identical profile cross-section is used at every point along the path. The
swept solid has the same shape from start to finish — like extruding pasta through
a die of fixed shape.

**Use this mode when:**
- You are sweeping a uniform cross-section (pipe, rod, channel, rail, wiring duct)
- The cross-section is not supposed to change at any point along the spine

**Watch out for:** Nothing — this is the correct mode for the vast majority of
sweeps.

#### Multisection

The profile morphs through a sequence of intermediate section sketches positioned
along the spine. The solid is interpolated between the start profile, each
intermediate section in path order, and the end of the spine.

**Use this mode when:**
- You need a shape that transitions from one cross-section to another along its
  length (e.g. round inlet → square outlet on a duct or nozzle)
- The transition must pass through specific intermediate cross-sections at
  defined positions — more controlled than Additive Loft's free-form blending

Sub-parameters active in Multisection mode:

| Sub-parameter | Type | Default | Description |
|---------------|------|---------|-------------|
| Add Section / Remove Section | Buttons | — | Toggle selection mode to add or remove intermediate section sketches from the list. |
| Section list | Ordered list | — | Sections in path order (top = spine start). Reorder by drag-and-drop. |

**Watch out for:** All intermediate sections must have the same number of closed
wires (loops) as the start profile. Only the very last section in the list may be
reduced to a single point (which closes the solid to a cone tip or similar).
Mismatching wire counts always causes a "Pipe could not be built" failure.
