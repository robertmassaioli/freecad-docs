# Assembly Tools

Complete index of all Assembly workbench tools.

---

## Assembly Setup

| Tool | Shortcut | Description |
|------|----------|-------------|
| [New Assembly](create-assembly.md#new-assembly) | `A` | Create a new assembly container |
| [Insert Component](create-assembly.md#insert-component) | — | Insert an existing part or sub-assembly as a link |
| [New Part](create-assembly.md#new-part) | — | Create a new empty part inside the assembly |
| Solve Assembly | — | Run the constraint solver to position all parts |

---

## Joints — Grounding

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Toggle Grounded](grounded.md) | `G` | Fix / unfix a part's position as the assembly reference |

---

## Joints — Kinematic

These joints define how parts move relative to each other.

| Joint | Shortcut | DOF | Description |
|-------|----------|-----|-------------|
| [Fixed](joints-kinematic.md#fixed-joint) | `F` | 0 | Lock two parts completely |
| [Revolute](joints-kinematic.md#revolute-joint) | `R` | 1 (rotation) | Allow rotation around a single axis |
| [Cylindrical](joints-kinematic.md#cylindrical-joint) | `C` | 2 (rot + trans) | Allow rotation and translation along a single axis |
| [Slider](joints-kinematic.md#slider-joint) | `S` | 1 (translation) | Allow linear movement along a single axis only |
| [Ball](joints-kinematic.md#ball-joint) | `B` | 3 (rotation) | Allow free rotation about a single point |

---

## Joints — Geometric

These joints constrain orientation and distance without specifying a single motion axis.

| Joint | Shortcut | Description |
|-------|----------|-------------|
| [Distance](joints-geometric.md#distance-joint) | `D` | Maintain a fixed distance or separation |
| [Parallel](joints-geometric.md#parallel-joint) | `N` | Make two axes parallel |
| [Perpendicular](joints-geometric.md#perpendicular-joint) | `M` | Make two axes perpendicular |
| [Angle](joints-geometric.md#angle-joint) | `X` | Fix the angle between two axes |

---

## Joints — Coupled Motion

These joints link the motion of two independently-jointed parts.

| Joint | Shortcut | Description |
|-------|----------|-------------|
| [Rack and Pinion](joints-coupled.md#rack-and-pinion-joint) | `Q` | Link a slider to a revolute (rack-and-pinion mechanism) |
| [Screw](joints-coupled.md#screw-joint) | `W` | Link a slider to a revolute (lead-screw mechanism) |
| [Gears](joints-coupled.md#gears-joint) | `T` | Link two revolutes with inverse rotation |
| [Belt](joints-coupled.md#belt-joint) | `L` | Link two revolutes with same rotation direction |

---

## Documentation and Output

| Tool | Description |
|------|-------------|
| Exploded View | Create an exploded-view animation of the assembly |
| Simulation | Drive joints through a range of motion and record the animation |
| Bill of Materials | Generate a parts list table |
| Export ASMT | Export the assembly to an ASMT file for external solvers |
