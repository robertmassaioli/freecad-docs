# Building Elements

Building elements are the parametric components that make up a BIM model —
walls, floors, columns, beams, doors, windows, and MEP (mechanical, electrical,
plumbing) components. Each element maps to an IFC class.

---

## Wall {#wall}

**Menu:** 3D/BIM → Wall  
**Command:** `Arch_Wall`  
**IFC class:** `IfcWall` / `IfcWallStandardCase`

A Wall is a vertical or near-vertical partition. It can be created from:

- **A line or wire** — wall follows the line/wire, thickness applied perpendicular
- **A face** — wall is extruded from the face normal
- **Existing geometry** — wraps the shape

### Key properties

| Property | Description |
|----------|-------------|
| `Width` | Wall thickness (mm). Default 200 mm. |
| `Height` | Wall height (mm). |
| `Align` | Which side of the reference line the thickness grows: Left, Right, or Center |
| `Normal` | Direction of height growth. Default (0,0,1) = vertical. |
| `Offset` | Shift from reference line along wall thickness direction |

### Compound walls

Multiple walls can be joined with **Arch → Add** to create T-junctions and
L-corners with proper topology. Use **Arch → Remove** to subtract openings.

---

## Curtain Wall {#curtain-wall}

**Menu:** 3D/BIM → Curtain Wall  
**Command:** `Arch_CurtainWall`  
**IFC class:** `IfcCurtainWall`

A Curtain Wall is a non-structural facade system made of a grid of panels
(typically glass). FreeCAD creates it from a base surface, dividing it into
a grid of mullions and panels.

### Key properties

| Property | Description |
|----------|-------------|
| `VerticalMullionNumber` | Number of vertical divisions |
| `HorizontalMullionNumber` | Number of horizontal divisions |
| `MullionSize` | Width of mullion members |
| `PanelThickness` | Thickness of infill panels |

---

## Column {#column}

**Menu:** 3D/BIM → Column  
**Command:** `BIM_Column`  
**IFC class:** `IfcColumn`

Creates a vertical structural column at a specified base point. The cross-section
is defined by selecting a **Profile** (from the profile library or a custom
sketch). The height and placement are set interactively.

### Step-by-step

1. Go to **3D/BIM → Column**.
2. Click in the 3-D view to place the column base.
3. In the task panel, set profile, height, and rotation.

---

## Beam {#beam}

**Menu:** 3D/BIM → Beam  
**Command:** `BIM_Beam`  
**IFC class:** `IfcBeam`

Creates a structural beam between two clicked points. The profile is chosen
from the profile library.

---

## Slab {#slab}

**Menu:** 3D/BIM → Slab  
**Command:** `BIM_Slab`  
**IFC class:** `IfcSlab`

Creates a horizontal floor or ceiling slab from a planar shape (face or
closed wire). The shape is offset vertically by the slab thickness.

### Key properties

| Property | Description |
|----------|-------------|
| `Thickness` | Slab thickness (mm) |
| `Normal` | Direction of thickness growth |

---

## Door {#door}

**Menu:** 3D/BIM → Door  
**Command:** `BIM_Door`  
**IFC class:** `IfcDoor`

Inserts a parametric door into an existing wall. A door must be placed on a
wall face — it automatically cuts an opening through the wall.

The door is defined by a **preset** (chosen from the door library) that
specifies the panel type, hinge side, and frame profile.

### Key properties

| Property | Description |
|----------|-------------|
| `Width` | Door leaf width |
| `Height` | Door opening height |
| `Frame` | Frame width |
| `Hosts` | The wall the door is inserted in |

---

## Window {#window}

**Menu:** 3D/BIM → Window  
**Command:** `Arch_Window`  
**IFC class:** `IfcWindow`

Inserts a parametric window (or door) into a wall. Similar to Door but with
more glazing-panel presets.

Windows can also be created from a sketch — draw the opening profile on a
wall face in the Sketcher, then run the Window command.

### Key properties

| Property | Description |
|----------|-------------|
| `Width`, `Height` | Overall window size |
| `Frame` | Frame width |
| `Louvre` | Louvre width (for louvre windows) |
| `Hosts` | The host wall |

---

## Stairs {#stairs}

**Menu:** 3D/BIM → Stairs  
**Command:** `Arch_Stairs`  
**IFC class:** `IfcStair`

Creates a parametric stair flight. FreeCAD computes the number of risers and
treads from the storey height and user-specified dimensions.

### Key properties

| Property | Description |
|----------|-------------|
| `Width` | Stair flight width |
| `Height` | Total rise height |
| `Nosing` | Tread nosing overhang |
| `TreadThickness` | Thickness of each tread |
| `RiserHeight` | Computed or fixed riser height |
| `NumberOfSteps` | Computed or fixed number of steps |
| `Landings` | None, At top, At bottom, At top and bottom |

---

## Roof {#roof}

**Menu:** 3D/BIM → Roof  
**Command:** `Arch_Roof`  
**IFC class:** `IfcRoof`

Creates a roof surface from a closed wire defining the roof outline. Each
edge of the wire becomes a roof slope, with a separate pitch angle and overhang.

### Key properties

| Property | Description |
|----------|-------------|
| `Angles` | Pitch angle per edge (list, degrees) |
| `Run` | Horizontal run distance per edge |
| `IdRel` | Ridge-related edge index (for hip roofs) |
| `Thickness` | Roof slab thickness |
| `Overhang` | Eave overhang distance |

---

## Panel {#panel}

**Menu:** 3D/BIM → Panel  
**Command:** `Arch_Panel`  
**IFC class:** `IfcPlate`

Creates a flat panel element — a thin planar solid. Used for cladding
panels, floor tiles, prefabricated wall panels, and similar thin elements.

### Key properties

| Property | Description |
|----------|-------------|
| `Thickness` | Panel thickness |
| `Length`, `Width` | Dimensions if created without a base shape |

---

## Frame {#frame}

**Menu:** 3D/BIM → Frame  
**Command:** `Arch_Frame`  
**IFC class:** `IfcMember`

Creates a structural frame member by sweeping a profile along a wire skeleton.
The skeleton can be any wire object (Draft Wire, edge, etc.).

---

## Fence {#fence}

**Menu:** 3D/BIM → Fence  
**Command:** `Arch_Fence`  
**IFC class:** `IfcFence`

Creates a fence by repeating a post element and connecting them with rail
elements along a path wire.

### Key properties

| Property | Description |
|----------|-------------|
| `Post` | Part shape used as the fence post |
| `Guard` | Part shape used as the fence rail/guard |
| `Spacing` | Distance between posts |

---

## Truss {#truss}

**Menu:** 3D/BIM → Truss  
**Command:** `Arch_Truss`  
**IFC class:** `IfcMember`

Creates a structural truss from a line, automatically generating the chord,
verticals, and diagonals.

### Key properties

| Property | Description |
|----------|-------------|
| `Height` | Truss depth |
| `TrussAngle` | Diagonal angle |
| `SlantType` | Diagonal pattern: Pratt, Warren, Howe, etc. |

---

## Equipment {#equipment}

**Menu:** 3D/BIM → Equipment  
**Command:** `Arch_Equipment`  
**IFC class:** `IfcFurnishingElement` / `IfcFlowTerminal`

A generic container for furniture, MEP equipment, or any object that does
not fit a more specific class. The visual geometry is a linked Part shape.

---

## Rebar {#rebar}

**Menu:** 3D/BIM → Rebar  
**Command:** `Arch_Rebar`  
**IFC class:** `IfcReinforcingBar`

Creates a reinforcing bar inside a structural element (wall, column, beam,
or slab). The bar path is defined by a sketch drawn on a face of the host element.

!!! note
    For detailed rebar layouts, the **Reinforcement workbench** (an external
    addon) provides bar shape families (straight, stirrup, L-bar, U-bar, etc.)
    that build on the basic Rebar object.

---

## Pipe {#pipe}

**Menu:** 3D/BIM → Pipe  
**Command:** `Arch_Pipe`  
**IFC class:** `IfcPipeSegment`

Creates a circular pipe along a Draft Wire or edge. Used for MEP plumbing,
HVAC ductwork (round), and drainage modelling.

### Key properties

| Property | Description |
|----------|-------------|
| `Diameter` | Pipe outer diameter (mm) |
| `WallThickness` | Pipe wall thickness |
| `Base` | Reference wire for the pipe centreline |

---

## Pipe Connector {#pipe-connector}

**Menu:** 3D/BIM → Pipe Connector  
**Command:** `Arch_PipeConnector`  
**IFC class:** `IfcPipeFitting`

Creates a fitting (elbow, tee, or reducer) at the junction of two or three pipes.
Select the pipes to join, then run the command.

---

## Common mistakes and pitfalls

!!! warning "Wall does not appear at the right height"
    **Cause:** The wall's reference line is drawn at Z = 0 (global) but the
    Level's elevation shifts the storey up. The wall is inside the Level
    container so its placement is relative to the Level.  
    **Fix:** When drawing on an elevated storey, set the working plane to the
    storey's elevation before drawing.

!!! warning "Door or Window does not cut through the wall"
    **Cause:** The door/window must be placed while its face is touching the
    wall face. If the door is free-floating (not hosted by a wall), it creates
    no opening.  
    **Fix:** Select the host wall first, then run the Door/Window command, or
    set the `Hosts` property to the wall.

!!! warning "Roof pitch looks wrong"
    **Cause:** The `Angles` property is a list of one angle per edge. If fewer
    angles are provided than edges, the last angle repeats.  
    **Fix:** Check that `len(Angles) == len(wire.Edges)` and set individual
    angles in the Properties panel.

---

## See also

- [Structure and IFC Hierarchy](structure.md) — where elements live in the hierarchy
- [IFC Management](ifc.md) — IFC classes and property sets for elements
