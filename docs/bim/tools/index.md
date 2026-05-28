# BIM Tools

## Structure and IFC Hierarchy

| Tool | Command | Description |
|------|---------|-------------|
| [Site](structure.md#site) | `Arch_Site` | Top-level container; maps to IfcSite |
| [Building](structure.md#building) | `Arch_Building` | Building container; maps to IfcBuilding |
| [Level](structure.md#level) | `Arch_Level` | Floor/storey container; maps to IfcBuildingStorey |
| [Space](structure.md#space) | `Arch_Space` | Room or space; maps to IfcSpace |

## Building Elements

| Tool | Command | Description |
|------|---------|-------------|
| [Wall](elements.md#wall) | `Arch_Wall` | Parametric wall from a line, wire, or face |
| [Curtain Wall](elements.md#curtain-wall) | `Arch_CurtainWall` | Grid-based glazed facade system |
| [Column](elements.md#column) | `BIM_Column` | Vertical structural column |
| [Beam](elements.md#beam) | `BIM_Beam` | Horizontal or inclined structural beam |
| [Slab](elements.md#slab) | `BIM_Slab` | Floor or ceiling slab from a planar shape |
| [Door](elements.md#door) | `BIM_Door` | Door inserted into a wall |
| [Window](elements.md#window) | `Arch_Window` | Window or door opening in a wall |
| [Stairs](elements.md#stairs) | `Arch_Stairs` | Parametric stair flight |
| [Roof](elements.md#roof) | `Arch_Roof` | Roof surface from a wire outline |
| [Panel](elements.md#panel) | `Arch_Panel` | Flat panel element (wall panel, floor panel) |
| [Frame](elements.md#frame) | `Arch_Frame` | Profile-based frame from a wire skeleton |
| [Fence](elements.md#fence) | `Arch_Fence` | Linear fence from a path + post profile |
| [Truss](elements.md#truss) | `Arch_Truss` | Structural truss from a line |
| [Equipment](elements.md#equipment) | `Arch_Equipment` | Generic equipment object (furniture, MEP) |
| [Rebar](elements.md#rebar) | `Arch_Rebar` | Reinforcement bar inside a structural element |
| [Pipe](elements.md#pipe) | `Arch_Pipe` | MEP pipe along a wire |
| [Pipe Connector](elements.md#pipe-connector) | `Arch_PipeConnector` | Elbow, tee, or transition at pipe junctions |

## Annotation and Documentation

| Tool | Command | Description |
|------|---------|-------------|
| [Text](annotation.md#text) | `BIM_Text` | 3-D text annotation |
| [Dimension (Aligned)](annotation.md#dimensions) | `BIM_DimensionAligned` | Dimension along the measured distance |
| [Dimension (Horizontal)](annotation.md#dimensions) | `BIM_DimensionHorizontal` | Horizontal dimension |
| [Dimension (Vertical)](annotation.md#dimensions) | `BIM_DimensionVertical` | Vertical dimension |
| [Leader](annotation.md#leader) | `BIM_Leader` | Leader line with text |
| [Axis](annotation.md#axis) | `Arch_Axis` | Construction axis line |
| [Axis System](annotation.md#axis-system) | `Arch_AxisSystem` | Grid of axes |
| [Grid](annotation.md#grid) | `Arch_Grid` | 2-D grid object |
| [Section Plane](annotation.md#section-plane) | `Arch_SectionPlane` | Cutting plane for 2-D view generation |
| [Drawing Page](annotation.md#drawing-page) | `BIM_TDPage` | TechDraw page for construction drawings |
| [Drawing View](annotation.md#drawing-view) | `BIM_TDView` | Add a BIM view to a TechDraw page |

## IFC Management

| Tool | Command | Description |
|------|---------|-------------|
| [IFC Elements](ifc.md#ifc-elements) | `BIM_IfcElements` | Manage IFC classes of document objects |
| [IFC Quantities](ifc.md#ifc-quantities) | `BIM_IfcQuantities` | Manage IFC quantity sets |
| [IFC Properties](ifc.md#ifc-properties) | `BIM_IfcProperties` | Manage IFC property sets |
| [Classification](ifc.md#classification) | `BIM_Classification` | Assign classification codes (Uniclass, OmniClass) |
| [Material](ifc.md#material) | `BIM_Material` | Assign and manage materials |
| [Preflight](ifc.md#preflight) | `BIM_Preflight` | Check model before IFC export |
| [Schedule](ifc.md#schedule) | `Arch_Schedule` | Generate quantity take-off schedules |
