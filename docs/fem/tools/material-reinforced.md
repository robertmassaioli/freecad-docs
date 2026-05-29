# Reinforced Material (Concrete)

> **In one sentence:** Define a composite concrete-and-rebar material for
> reinforced concrete structural analysis with CalculiX shell elements.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Materials → Reinforced Material (Concrete)

Defines a composite material for reinforced concrete: a matrix material
(concrete) combined with a reinforcement material (steel rebar fibres). The
two materials are combined with a specified rebar angle (direction of
reinforcement fibres relative to global axes).

---

## Components

| Component | Description |
|-----------|-------------|
| Matrix material | Concrete — compression-dominant, brittle |
| Reinforcement material | Steel rebar — tension-carrying fibres |
| Rebar angle | Direction of reinforcement fibres relative to global axes |

Supported by CalculiX for shell-based reinforced concrete structures.

---

## See also

- [Material for Solid](material-solid.md) — base material type
- [Shell Thickness](shell-thickness.md) — shell element thickness for concrete slabs
- [FEM Workbench](../index.md) — workbench overview
