# Assembly Tools

Complete reference index for all Assembly workbench tools, grouped by function.
Each tool name links to its dedicated page.

---

## Assembly Setup

Tools for creating the assembly container and populating it with parts.

| Tool | Shortcut | Description |
|------|----------|-------------|
| [New Assembly](new-assembly.md) | `A` | Create a new assembly container |
| [Insert Component](insert-component.md) | — | Insert an existing part or sub-assembly as a live link |
| [New Part](new-part.md) | — | Create a new empty body inside the assembly for in-context modelling |

---

## Grounding

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Toggle Grounded](grounded.md) | `G` | Fix one part in space as the assembly's immovable reference |

---

## Joints — Kinematic

Define how many and which degrees of freedom two parts share.

| Joint | Shortcut | DOF remaining | Description |
|-------|----------|---------------|-------------|
| [Fixed](joint-fixed.md) | `F` | 0 | Lock two parts completely — no relative motion |
| [Revolute](joint-revolute.md) | `R` | 1 (rotation) | Allow rotation around a single shared axis |
| [Cylindrical](joint-cylindrical.md) | `C` | 2 (rot + trans) | Allow rotation and translation along a single axis, independently |
| [Slider](joint-slider.md) | `S` | 1 (translation) | Allow linear motion along a single axis only |
| [Ball](joint-ball.md) | `B` | 3 (all rotations) | Allow free rotation about a single point |

---

## Joints — Geometric

Constrain orientation and distance without prescribing a specific motion axis.

| Joint | Shortcut | Description |
|-------|----------|-------------|
| [Distance](joint-distance.md) | `D` | Maintain a fixed gap or separation between two elements |
| [Parallel](joint-parallel.md) | `N` | Force two axes or face normals to be parallel |
| [Perpendicular](joint-perpendicular.md) | `M` | Force two axes or face normals to be exactly 90° apart |
| [Angle](joint-angle.md) | `X` | Fix the angle between two axes to an arbitrary value |

---

## Joints — Coupled Motion

Link the motion of two independently-jointed parts by a ratio.

| Joint | Shortcut | Description |
|-------|----------|-------------|
| [Rack and Pinion](joint-rack-pinion.md) | `Q` | Couple a Revolute (pinion) to a Slider (rack) |
| [Screw](joint-screw.md) | `W` | Couple a Revolute to a Slider on the same axis by a pitch ratio |
| [Gears](joint-gears.md) | `T` | Couple two Revolutes with opposite-direction rotation |
| [Belt](joint-belt.md) | `L` | Couple two Revolutes with same-direction rotation |

---

## Solving

| Tool | Description |
|------|-------------|
| [Solve Assembly](solve.md) | Run the constraint solver to position all parts |

---

## Documentation and Output

| Tool | Description |
|------|-------------|
| [Exploded View](exploded-view.md) | Define and animate an exploded assembly view |
| [Simulation](simulation.md) | Drive joints through a range of motion for kinematic animation |
| [Bill of Materials](bom.md) | Generate a spreadsheet listing all components and quantities |
| [Export ASMT](export-asmt.md) | Export the assembly kinematic structure to an ASMT file |
