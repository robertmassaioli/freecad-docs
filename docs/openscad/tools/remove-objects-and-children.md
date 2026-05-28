# Remove Objects and Children

> **In one sentence:** Delete the selected objects and every object in their
> subtree that is not referenced by anything outside the subtree.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Remove Objects and Children  
**Shortcut:** none

A smarter delete: removes the selected objects and recursively deletes all
child objects that would become orphans — objects that are no longer
referenced by any remaining object in the document. Objects that are shared
(referenced by something outside the deleted subtree) are preserved.

---

## Intuition

Pressing Delete in FreeCAD only removes the top-level object; its children
often become orphaned objects that clutter the document tree. Remove Objects
and Children cleans up the entire branch at once — like pruning a dead branch
from a tree all the way back to where it connects to healthy wood.

---

## When to use it

- You tried a boolean operation that failed and want to remove the failed
  boolean along with the temporary shapes it created.
- You are discarding an experimental branch of the modelling tree.
- You want to clean up the document after replacing objects.

---

## When NOT to use it

- **When child objects are shared** — shared children are not deleted. This is
  intentional and safe behaviour; you don't need to worry about accidentally
  deleting objects still in use.

---

## Step-by-step

1. Select the root object(s) of the subtree to delete.
2. Choose **OpenSCAD → Remove Objects and Children**.
3. FreeCAD deletes the selected objects and all unreferenced children.

---

## Parameters

No parameters — operates on the current selection.

---

## Common mistakes and pitfalls

!!! warning "Expected child was not deleted"
    **Cause:** The child is referenced by another object outside the deleted
    subtree, so it is preserved.  
    **Fix:** This is correct behaviour. Delete the other referencing object
    first if you also want the child removed.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_RemoveSubtree', 0)")
```

---

## See also

- [Replace Object](replace-object.md) — swap rather than delete
- [OpenSCAD Workbench](../index.md) — workbench overview
