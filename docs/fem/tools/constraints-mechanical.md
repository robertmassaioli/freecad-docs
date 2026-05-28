# Mechanical Boundary Conditions and Loads

> **In one sentence:** Mechanical boundary conditions fix how a structure can
> move, and mechanical loads apply the forces, pressures, and body loads that
> drive the deformation the solver computes.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions and Loads

| Tool | Type | Description |
|------|------|-------------|
| [Fixed Constraint](#fixed-constraint) | BC | Fix all DOF on a face/edge/vertex |
| [Rigid Body Constraint](#rigid-body-constraint) | BC | Constrain a face to move as one rigid body |
| [Displacement Constraint](#displacement-constraint) | BC | Prescribe specific displacements / rotations |
| [Contact Constraint](#contact-constraint) | BC | Contact between two faces |
| [Tie Constraint](#tie-constraint) | BC | Bond non-touching faces together |
| [Spring Constraint](#spring-constraint) | BC | Attach a linear spring to a face |
| [Force Load](#force-load) | Load | Apply a total or distributed force |
| [Pressure Load](#pressure-load) | Load | Apply a normal pressure to a face |
| [Centrifugal Load](#centrifugal-load) | Load | Centrifugal body load from rotation |
| [Self Weight (Gravity)](#self-weight-gravity) | Load | Gravitational body load |

---

## Intuition

### Boundary conditions vs loads

**Boundary conditions (BCs)** tell the solver where the structure is held —
they prevent rigid body motion and define the reference frame. Without at
least one BC, the structure is free to translate and rotate, and the linear
system of equations has no unique solution (singular stiffness matrix).

**Loads** tell the solver what forces or displacements are *applied*. They
are the input that drives the deformation.

The most common structural analysis setup is:

```
Fixed Constraint on base face  ← "this face is bolted to the wall"
Force Load on top face         ← "this face carries 1000 N upward"
→ Solve → Get stresses and displacements
```

### Choosing where to apply BCs

Apply boundary conditions to the geometry that physically represents the
constraint:

- The face where the part is bolted to a fixed frame → **Fixed Constraint**
- The face where a shaft can slide in a bearing but not translate → **Displacement Constraint** (translate = free, rotate = constrained)
- The face where two mating parts touch → **Contact Constraint** or **Tie**

Misapplied BCs are the most common source of unrealistic FEM results.
If the deformed shape looks physically implausible, check the BCs first.

---

## Fixed Constraint

**Menu:** FEM → Model → Mechanical Boundary Conditions → Fixed Constraint

Prevents all translational and rotational motion on the selected geometric
entity. All six DOF are set to zero. This is the simplest and most commonly
used constraint.

### Step-by-step

1. Select a face, edge, or vertex in the 3-D view or model tree.
2. Choose **FEM → Model → Mechanical BCs → Fixed Constraint**.
3. The constraint appears in the analysis with a visual indicator on the
   selected geometry.

### When to use Fixed vs Displacement

- **Fixed** — use when the surface truly cannot move at all: bolted base,
  welded joint to a rigid wall.
- **Displacement** — use when some motion is allowed (e.g. the surface can
  slide in one direction but not others, or a prescribed non-zero displacement
  is needed).

### Python API

```python
import ObjectsFem

doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

fix = ObjectsFem.makeConstraintFixed(doc, "ConstraintFixed")
fix.References = [(doc.getObject("Body"), "Face1")]
analysis.addObject(fix)
doc.recompute()
```

---

## Rigid Body Constraint

**Menu:** FEM → Model → Mechanical Boundary Conditions → Rigid Body Constraint

Constrains all nodes on a face to move together as a rigid body — no relative
deformation between them. The face as a whole can still translate and rotate
(unless other constraints prevent it).

### When to use it

- **Load distribution:** Apply a force or moment to a face that should be
  distributed realistically across the face without the face itself
  deforming (avoids stress concentrations at load application points).
- **Stiff attachment:** Model a stiff connector or bracket face that does
  not flex.

### Reference point

The rigid body motion is tied to a **reference point** (a node not on the
face, or a defined point) that carries the resultant force and moment. The
solver internally computes the nodal forces on the face consistent with the
applied load at the reference point.

---

## Displacement Constraint

**Menu:** FEM → Model → Mechanical Boundary Conditions → Displacement Constraint

Prescribes the displacement (and/or rotation) of a face, edge, or vertex to
a specific value — including zero (fixed in that direction) or a non-zero
prescribed deformation.

### Parameters

Each of the six DOF can be independently:
- **Free** — not constrained.
- **Fixed (= 0)** — displacement or rotation is zero in this direction.
- **Prescribed** — displacement or rotation has a specific non-zero value.

| DOF | Meaning | Example use |
|-----|---------|-------------|
| x | Translation in X | Fix X only: allows Y,Z sliding |
| y | Translation in Y | |
| z | Translation in Z | Fix Z only: a roller support |
| Rx | Rotation about X | Fix rotation on a pinned joint |
| Ry | Rotation about Y | |
| Rz | Rotation about Z | |

### Example: Roller support (slides in X, fixed in Y and Z)

- x: **Free**
- y: **Fixed (0)**
- z: **Fixed (0)**
- Rx, Ry, Rz: **Free** (unless rotations must also be constrained)

### Prescribed displacement (non-zero)

Set the displacement value to a non-zero number to simulate a forced
deformation — e.g. a thermal press fit where one face is pushed by 0.1 mm.

---

## Contact Constraint

**Menu:** FEM → Model → Mechanical Boundary Conditions → Contact Constraint

Defines a contact pair between two faces that may or may not be touching.
When the analysis runs, the solver enforces that the faces cannot overlap
(non-penetration) and optionally enforces friction.

### Contact types

| Type | Penetration | Separation | Friction |
|------|-------------|------------|---------|
| Frictionless | Prevented | Allowed | No |
| Rough (no slip) | Prevented | Allowed | Full stick |
| Coulomb friction | Prevented | Allowed | μ coefficient |

### Step-by-step

1. Select the **master face** (usually the stiffer body).
2. Ctrl-click the **slave face** (usually the more compliant body).
3. Choose **FEM → Model → Mechanical BCs → Contact Constraint**.
4. Set the friction coefficient (0 for frictionless).
5. Click **OK**.

### Note on nonlinear solve

Contact is inherently nonlinear — the contact area changes as the load
increases. The CalculiX solver must use its nonlinear solver (geometric or
material nonlinearity enabled) for contact problems. Linear static analysis
does not support contact.

---

## Tie Constraint

**Menu:** FEM → Model → Mechanical Boundary Conditions → Tie Constraint

Bonds two faces together by tying corresponding nodes — the two faces move
as one even if they do not overlap or are not initially touching. Unlike
Contact, Tie is always active and allows no separation or sliding.

### When to use Tie vs Contact

| Situation | Use |
|-----------|-----|
| Two faces that are always fully bonded (glued, welded, brazed) | Tie |
| Two faces that may separate or slide | Contact |
| Mesh transitions between sub-models with different mesh density | Tie |

### Tied mesh connections

Tie is commonly used to join two mesh regions with different element sizes —
for example, a refined mesh region around a notch tied to a coarser mesh of
the rest of the body. The tie constraint interpolates between the coarse and
fine meshes automatically.

---

## Spring Constraint

**Menu:** FEM → Model → Mechanical Boundary Conditions → Spring Constraint

Adds a linear elastic spring (or set of springs) to a face, edge, or vertex.
The spring exerts a force proportional to the displacement: F = k × d.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Normal stiffness | Spring stiffness in the face-normal direction (N/mm) |
| Tangential stiffness | Spring stiffness parallel to the face (N/mm) |
| Reference edge | Optional: sets the spring direction |

### Use cases

- Simulating a flexible foundation (soil, rubber mount, compliant support).
- Modelling a structure supported by springs rather than rigidly fixed.
- Representing the stiffness of adjacent structure without meshing it
  (foundation stiffness method).

---

## Force Load

**Menu:** FEM → Model → Mechanical Loads → Force Load

Applies a concentrated or distributed force to a face, edge, or vertex.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Force | Total force magnitude (N) |
| Direction | Force vector direction (X, Y, Z components or selected edge/face normal) |
| References | Face(s), edge(s), or vertex/vertices to load |
| Reversed | Flip the direction of the force |

### Total force vs pressure

- **Force Load** applies a **total force** distributed over the selected
  face. If you select a larger face, the same total force is spread over
  a larger area (lower surface pressure).
- **Pressure Load** applies a **force per unit area** — the total force
  scales with the face area.

Choose Force Load when you know the total applied force (e.g. the bolt
pretension is 5000 N regardless of the bolt head area). Choose Pressure
when you know the pressure (e.g. fluid pressure at 10 bar).

### Python API

```python
import ObjectsFem

force = ObjectsFem.makeConstraintForce(doc, "Force")
force.Force = 1000.0   # N
force.Direction = (doc.getObject("Body"), ["Face3"])
force.Reversed = False
force.References = [(doc.getObject("Body"), "Face5")]
analysis.addObject(force)
doc.recompute()
```

---

## Pressure Load

**Menu:** FEM → Model → Mechanical Loads → Pressure Load

Applies a uniform pressure (N/mm² = MPa) perpendicular to selected faces.
The direction is always the face inward normal (positive pressure compresses
the face inward). Use a negative value for suction (outward tension).

### Common values

| Situation | Pressure |
|-----------|---------|
| 1 bar (100 kPa) water pressure | 0.1 MPa |
| 10 bar hydraulic system | 1.0 MPa |
| 100 psi (typical tire) | 0.69 MPa |
| Atmospheric (101.3 kPa) | 0.1013 MPa |

---

## Centrifugal Load

**Menu:** FEM → Model → Mechanical Loads → Centrifugal Load

Applies centrifugal (centripetal) body loads to a rotating structure. The
body load is computed from the rotational speed and the distance of each
element from the rotation axis.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Rotation Axis | A line or edge defining the rotation axis |
| Rotation Frequency | Angular velocity in rpm or Hz |

### Use cases

- Rotating machinery: turbine blades, compressor discs, flywheels.
- Spinning shafts: calculate hoop stress from rotation.
- Centrifuge rotors.

**Density must be set** in the material — centrifugal load is a body force
proportional to mass.

---

## Self Weight (Gravity)

**Menu:** FEM → Model → Mechanical Loads → Self Weight (Gravity)

Applies gravitational acceleration as a body force to the entire analysis.
The gravity vector is applied uniformly (default: 9.81 m/s² in the –Z
direction).

### Parameters

| Parameter | Description |
|-----------|-------------|
| Gravity Acceleration | Magnitude (default 9810 mm/s²) |
| Direction | Usually (0, 0, −1) for downward |

**Density must be set** in the material — self-weight is proportional to
mass. Without density, the gravitational load is zero.

### Python API

```python
import ObjectsFem

grav = ObjectsFem.makeConstraintSelfWeight(doc, "SelfWeight")
grav.Gravity_x = 0.0
grav.Gravity_y = 0.0
grav.Gravity_z = -9.81   # m/s²
analysis.addObject(grav)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "No boundary condition → singular stiffness matrix"
    Every structural analysis must have at least one Fixed or Displacement
    constraint to prevent rigid body motion. If the solver fails with a
    singular stiffness matrix error, check that a constraint has been applied
    and that its reference geometry still exists.

!!! warning "Contact requires nonlinear solver"
    Adding a Contact constraint but running CalculiX in linear static mode
    will either fail or ignore the contact. Set the CalculiX analysis type
    to `static` with nonlinear geometry enabled before solving.

!!! warning "Force vs Pressure direction"
    Pressure is always face-normal. Force requires a direction vector. If
    the force direction is left as the face normal and the face curves, the
    force direction varies across the face — this is physically correct for
    distributed pressure but may not be what you intend for a concentrated
    force. Set an explicit global direction vector for most force loads.

!!! warning "Centrifugal and gravity loads need density"
    Both centrifugal and self-weight loads are proportional to mass
    (ρ × V × a). If the material density is zero or not set, these loads
    produce zero force and the results will be misleading rather than wrong.

---

## See also

- [Materials](materials.md) — density required for inertial loads
- Thermal Constraints and Loads — temperature BCs and heat loads
- Mesh from Shape — the mesh must cover the boundary condition geometry
- Solver CalculiX — solver settings for nonlinear contact and large deformation
