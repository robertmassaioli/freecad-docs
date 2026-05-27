```python
import FreeCAD as App
import Sketcher

doc = App.newDocument()
sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
doc.recompute()
```
