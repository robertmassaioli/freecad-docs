# BIM Tools

## Structure and IFC Hierarchy

| Tool | Command | Description |
|------|---------|-------------|
| [Site](site.md) | `Arch_Site` | Top-level container; maps to IfcSite |
| [Building](building.md) | `Arch_Building` | Building container; maps to IfcBuilding |
| [Level](level.md) | `Arch_Level` | Floor/storey container; maps to IfcBuildingStorey |
| [Space](space.md) | `Arch_Space` | Room or space; maps to IfcSpace |

## Building Elements

| Tool | Command | Description |
|------|---------|-------------|
| [Wall](wall.md) | `Arch_Wall` | Parametric wall from a line, wire, or face |
| [Curtain Wall](curtain-wall.md) | `Arch_CurtainWall` | Grid-based glazed facade system |
| [Column](column.md) | `BIM_Column` | Vertical structural column |
| [Beam](beam.md) | `BIM_Beam` | Horizontal or inclined structural beam |
| [Slab](slab.md) | `BIM_Slab` | Floor or ceiling slab from a planar shape |
| [Door](door.md) | `BIM_Door` | Door inserted into a wall |
| [Window](window.md) | `Arch_Window` | Window or door opening in a wall |
| [Stairs](stairs.md) | `Arch_Stairs` | Parametric stair flight |
| [Roof](roof.md) | `Arch_Roof` | Roof surface from a wire outline |
| [Panel](panel.md) | `Arch_Panel` | Flat panel element (wall panel, floor panel) |
| [Frame](frame.md) | `Arch_Frame` | Profile-based frame from a wire skeleton |
| [Fence](fence.md) | `Arch_Fence` | Linear fence from a path + post profile |
| [Truss](truss.md) | `Arch_Truss` | Structural truss from a line |
| [Equipment](equipment.md) | `Arch_Equipment` | Generic equipment object (furniture, MEP) |
| [Rebar](rebar.md) | `Arch_Rebar` | Reinforcement bar inside a structural element |
| [Pipe](pipe.md) | `Arch_Pipe` | MEP pipe along a wire |
| [Pipe Connector](pipe-connector.md) | `Arch_PipeConnector` | Elbow, tee, or transition at pipe junctions |

## Annotation and Documentation

| Tool | Command | Description |
|------|---------|-------------|
| [Text](text.md) | `BIM_Text` | 3-D text annotation |
| [Dimension (Aligned)](dimension-aligned.md) | `BIM_DimensionAligned` | Dimension along the measured distance |
| [Dimension (Horizontal)](dimension-horizontal.md) | `BIM_DimensionHorizontal` | Horizontal dimension |
| [Dimension (Vertical)](dimension-vertical.md) | `BIM_DimensionVertical` | Vertical dimension |
| [Leader](leader.md) | `BIM_Leader` | Leader line with text |
| [Axis](axis.md) | `Arch_Axis` | Construction axis line |
| [Axis System](axis-system.md) | `Arch_AxisSystem` | Grid of axes |
| [Grid](grid.md) | `Arch_Grid` | 2-D grid object |
| [Section Plane](section-plane.md) | `Arch_SectionPlane` | Cutting plane for 2-D view generation |
| [Drawing Page](drawing-page.md) | `BIM_TDPage` | TechDraw page for construction drawings |
| [Drawing View](drawing-view.md) | `BIM_TDView` | Add a BIM view to a TechDraw page |

## IFC Management

| Tool | Command | Description |
|------|---------|-------------|
| [IFC Elements](ifc-elements.md) | `BIM_IfcElements` | Manage IFC classes of document objects |
| [IFC Quantities](ifc-quantities.md) | `BIM_IfcQuantities` | Manage IFC quantity sets |
| [IFC Properties](ifc-properties.md) | `BIM_IfcProperties` | Manage IFC property sets |
| [Classification](classification.md) | `BIM_Classification` | Assign classification codes (Uniclass, OmniClass) |
| [Material](material.md) | `BIM_Material` | Assign and manage materials |
| [Preflight](preflight.md) | `BIM_Preflight` | Check model before IFC export |
| [Schedule](schedule.md) | `Arch_Schedule` | Generate quantity take-off schedules |
