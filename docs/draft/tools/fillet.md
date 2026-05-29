# Fillet

> **In one sentence:** Creates a tangent fillet arc between two existing Draft
> lines that share or nearly share a corner endpoint.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Fillet  
**Command:** `Draft_Fillet`

Joins two lines with a circular arc of a given radius, trimming each line
back to the tangent point. The lines must share or nearly share an endpoint
(the corner to be filleted).

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Radius | Radius of the fillet arc |
| Delete original wires | Remove the original lines after filleting |

---

## Common mistakes and pitfalls

!!! warning "Lines too short for the radius"
    If the lines are shorter than the fillet radius allows, the fillet
    cannot be created. Reduce the radius or lengthen the lines.

---

## See also

- [Wire](wire.md) — polyline with built-in Fillet Radius property
- [Arc](arc.md) — place arcs manually
- [Draft Workbench](../index.md) — workbench overview
