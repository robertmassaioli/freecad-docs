# Rebar

> **In one sentence:** Create a reinforcing bar inside a structural element
> by drawing the bar path on a face of the host element.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Rebar  
**Command:** `Arch_Rebar`  
**IFC class:** `IfcReinforcingBar`

Creates a reinforcing bar inside a structural element (wall, column, beam,
or slab). The bar path is defined by a sketch drawn on a face of the host element.

!!! note "Reinforcement workbench"
    For detailed rebar layouts with bar shape families (straight, stirrup,
    L-bar, U-bar, etc.), the **Reinforcement workbench** (an external addon)
    builds on the basic Rebar object and is recommended for detailed structural
    reinforcement work.

---

## Step-by-step

1. Select the structural element (wall, column, slab, or beam) to reinforce.
2. Switch to the Sketcher and draw the bar path on a face of that element.
3. Return to BIM, select the sketch, and run **3D/BIM → Rebar**.
4. Set the rebar diameter and other properties.

---

## See also

- [Wall](wall.md) — typical rebar host element
- [Column](column.md) — typical rebar host element
- [Slab](slab.md) — typical rebar host element
- [BIM Workbench](../index.md) — workbench overview
