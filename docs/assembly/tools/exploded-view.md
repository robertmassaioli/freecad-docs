# Exploded View

> **In one sentence:** Create a step-by-step animated exploded view of the
> assembly — where each part moves apart along a path you define — for use
> in documentation, instructions, and presentations.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Exploded View  
**Shortcut:** none

---

## Intuition

An exploded view shows all parts of an assembly separated along radial or
directional paths so that each component is individually visible. The
Assembly workbench's Exploded View tool lets you:

1. Define where each part should move (the "exploded" position).
2. Order the moves into a sequence of steps.
3. Play through the sequence as an animation or step through it manually.
4. Project the exploded state into a TechDraw drawing.

The key difference from simply moving parts in the 3-D view is that
Exploded View is **non-destructive** — it stores the exploded positions
separately and does not change the joint definitions or the solved positions
of the assembly. Toggling "Collapsed" brings every part back to its
joint-consistent position instantly.

---

## Step-by-step

### Creating the exploded view

1. **Solve Assembly** first — parts must be in their assembled positions
   before defining the exploded moves.
2. Choose **Assembly → Exploded View**. The Exploded View task panel opens.
3. The panel shows a list of **Steps** (initially empty) and controls for
   adding moves.

### Adding a move step

1. In the task panel, click **Create Move**.
2. Select one or more parts in the model tree or 3-D view.
3. Use the manipulator (the arrow handles that appear on the part) to drag
   the part to its exploded position, or enter an exact distance in the
   task panel.
4. Click **End Move** to confirm the position. A step appears in the step
   list.
5. Repeat for each part or group of parts.

### Ordering steps

Steps are played in the order they appear in the list. Drag steps to
reorder them. A typical ordering explodes the outermost parts first, working
inward to the core component.

### Playing the animation

- **Play Forward** — animates all steps in order, parts moving to their
  exploded positions.
- **Play Backward** — reverses the animation (assembles the parts).
- **Step Forward / Backward** — advance one step at a time for
  presentation control.
- The playback speed can be adjusted in the task panel.

### Exiting the task panel

Click **OK** to close the task panel and keep the exploded view definition
stored in the model tree. The exploded view object appears under the assembly.
Double-click it to re-enter the task panel.

---

## Exploded state in TechDraw

After creating an exploded view, you can project it into a TechDraw drawing:

1. Open or create a TechDraw page.
2. Insert a view: **TechDraw → Views → Insert Assembly View** (or Insert View
   if the assembly is active).
3. In the view dialog, select the assembly and enable **Exploded** to project
   the exploded state.

The exploded view appears on the drawing sheet with the parts in their
separated positions.

---

## Managing exploded views

A document can contain multiple exploded view definitions (e.g. one for
a full assembly explode, one for a sub-assembly detail). Each is stored as
a separate object in the model tree. Delete the object to remove the
exploded view definition.

Changing the assembly (adding parts, re-solving, changing joint parameters)
does not automatically update the exploded view steps. If the assembly
changes significantly, re-enter the Exploded View task panel and adjust
the steps.

---

## Common mistakes and pitfalls

!!! warning "Solve before creating exploded view"
    If parts are not in their assembled positions when you define the
    exploded moves, the moves will be relative to whatever position the
    parts were in. Always run Solve Assembly first.

!!! warning "Exploded view does not survive part deletion"
    If a part referenced by an exploded view step is deleted from the
    assembly, the step becomes invalid. Remove invalid steps from the
    step list in the task panel.

!!! warning "Animation quality depends on step count"
    Each step is a direct linear interpolation from assembled to exploded
    position. For parts that need to travel around other parts to avoid
    intersection, create intermediate steps that first move the part clear
    of obstructions.

---

## Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

# Open the Exploded View task panel via GUI command
Gui.doCommand('import CommandCreateView')
Gui.doCommand('CommandCreateView.CommandCreateView().Activated()')

# Exploded view objects are stored as Assembly::AssemblyView objects.
# Programmatic creation of steps is not exposed in the public API;
# use the task panel UI for step definition.
doc = App.ActiveDocument
# List all exploded view objects in the assembly
asm = doc.getObject("Assembly")
exploded_views = [obj for obj in asm.OutList
                  if obj.TypeId == "Assembly::AssemblyView"]
```

---

## See also

- [Solve Assembly](solve.md) — solve the assembly before creating an exploded view
- Simulation — drive joint motion rather than creating fixed exploded steps
- Bill of Materials — generate a parts list to accompany the exploded view
