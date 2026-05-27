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
| Toggle Grounded | `G` | Fix / unfix a part's position as the assembly reference |

---

## Joints — Kinematic

These joints define how parts move relative to each other.

| Joint | Shortcut | DOF | Description |
|-------|----------|-----|-------------|
| Fixed | `F` | 0 | Lock two parts completely |
| Revolute | `R` | 1 (rotation) | Allow rotation around a single axis |
| Cylindrical | `C` | 2 (rot + trans) | Allow rotation and translation along a single axis |
| Slider | `S` | 1 (translation) | Allow linear movement along a single axis only |
| Ball | `B` | 3 (rotation) | Allow free rotation about a single point |

---

## Joints — Geometric

These joints constrain orientation and distance without specifying a single motion axis.

| Joint | Shortcut | Description |
|-------|----------|-------------|
| Distance | `D` | Maintain a fixed distance or separation |
| Parallel | `N` | Make two axes parallel |
| Perpendicular | `M` | Make two axes perpendicular |
| Angle | `X` | Fix the angle between two axes |

---

## Joints — Coupled Motion

These joints link the motion of two independently-jointed parts.

| Joint | Shortcut | Description |
|-------|----------|-------------|
| Rack and Pinion | `Q` | Link a slider to a revolute (rack-and-pinion mechanism) |
| Screw | `W` | Link a slider to a revolute (lead-screw mechanism) |
| Gears | `T` | Link two revolutes with inverse rotation |
| Belt | `L` | Link two revolutes with same rotation direction |

---

## Documentation and Output

| Tool | Description |
|------|-------------|
| Exploded View | Create an exploded-view animation of the assembly |
| Simulation | Drive joints through a range of motion and record the animation |
| Bill of Materials | Generate a parts list table |
| Export ASMT | Export the assembly to an ASMT file for external solvers |
