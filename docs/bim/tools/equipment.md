# Equipment

> **In one sentence:** Create a generic container for furniture, MEP
> equipment, or any object that does not fit a more specific IFC class.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Equipment  
**Command:** `Arch_Equipment`  
**IFC class:** `IfcFurnishingElement` / `IfcFlowTerminal`

A generic container for any building element — furniture, HVAC equipment,
plumbing fixtures, electrical equipment — that does not map to a more specific
IFC element type. The visual geometry is a linked Part shape.

---

## When to use it

- Furniture (desks, chairs, kitchen units).
- Mechanical, electrical, and plumbing (MEP) fixtures not covered by [Pipe](pipe.md).
- Any object you want to carry IFC properties and appear in schedules.

---

## See also

- [Pipe](pipe.md) — MEP piping elements
- [Rebar](rebar.md) — reinforcing bar inside structural elements
- [Schedule](schedule.md) — generate equipment schedules
- [BIM Workbench](../index.md) — workbench overview
