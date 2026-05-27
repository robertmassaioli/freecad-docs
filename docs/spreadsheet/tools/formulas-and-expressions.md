# Formulas and Expressions

> **In one sentence:** Every cell that starts with `=` is evaluated by
> FreeCAD's built-in expression engine — a formula language that supports
> arithmetic, mathematical functions, unit quantities, and references to any
> model property.

---

## Overview

**Workbench:** Spreadsheet (also available in constraint dialogs, Part Design
feature dialogs, and anywhere FreeCAD accepts an expression)

The expression engine is the shared infrastructure behind Spreadsheet formulas
and Part Design's "bind expression to parameter" feature. Understanding it once
gives you full power in all contexts.

---

## Intuition

FreeCAD's expression engine is similar to Excel formulas but with three key
differences:

1. **Units are first-class.** `200 mm + 0.5 in` is legal and evaluates to the
   correct sum. You do not need to manually convert; the engine handles unit
   arithmetic.
2. **References reach outside the spreadsheet.** You can write
   `=Box.Length` to read the length of a Part Box, or `=Pad.Length` to
   read a Part Design Pad's depth. The spreadsheet can be a live dashboard
   of model state.
3. **The formula bar is the same everywhere.** The same syntax works in a
   Sketch constraint dialog, a Pad length field, or a spreadsheet cell.

---

## Entering a formula

Type `=` as the first character in a cell. The cell switches to formula mode
and the content bar shows the expression. Press Enter to confirm.

```
=42
=A1 * 2
=sin(pi / 6)
=Box.Length + 10 mm
```

---

## Basic arithmetic

| Operator | Meaning | Example |
|----------|---------|---------|
| `+` | Addition | `=A1 + B1` |
| `-` | Subtraction | `=A1 - 5` |
| `*` | Multiplication | `=A1 * 2.5` |
| `/` | Division | `=A1 / B1` |
| `^` | Power | `=A1^2` |
| `%` | Modulo | `=A1 % 3` |

---

## Unit quantities

Append a unit symbol to any number. FreeCAD stores the value in its internal
SI base unit (mm, kg, s, …) and converts on display.

```
= 200 mm
= 3.14159 * (25 mm)^2    # area of a circle, radius 25 mm
= 100 mm + 0.5 in        # mixed units — legal
= 9.81 m/s^2             # acceleration
```

Common unit symbols:

| Category | Symbols |
|----------|---------|
| Length | `mm`, `cm`, `m`, `in`, `ft` |
| Angle | `deg`, `rad` |
| Mass | `kg`, `g`, `lb` |
| Time | `s`, `ms`, `min`, `h` |
| Temperature | `K`, `degC`, `degF` |
| Pressure | `Pa`, `MPa`, `psi` |
| Force | `N`, `kN`, `lbf` |

---

## Cell references

| Syntax | Meaning |
|--------|---------|
| `A1` | Cell at column A, row 1 (relative) |
| `$A$1` | Absolute reference — does not shift when copied |
| `A1:C3` | Range (for functions like `sum`, `count`) |
| `MyAlias` | Cell with alias `MyAlias` in the same sheet |

---

## Referencing other spreadsheets

If a document has a second spreadsheet named `Materials`:

```
= Materials.Density
= Materials.B3
```

---

## Mathematical functions

### Trigonometry

| Function | Description | Example |
|----------|-------------|---------|
| `sin(x)` | Sine (argument in radians or with unit) | `=sin(30 deg)` |
| `cos(x)` | Cosine | `=cos(pi / 3)` |
| `tan(x)` | Tangent | `=tan(45 deg)` |
| `asin(x)` | Arcsine, returns radians | `=asin(0.5)` |
| `acos(x)` | Arccosine | `=acos(0)` |
| `atan(x)` | Arctangent | `=atan(1)` |
| `atan2(y, x)` | Two-argument arctangent | `=atan2(3, 4)` |

### Exponential and logarithm

| Function | Description |
|----------|-------------|
| `exp(x)` | e^x |
| `log(x)` | Natural logarithm (base e) |
| `log(x, b)` | Logarithm base b |
| `pow(x, y)` | x raised to the power y |
| `sqrt(x)` | Square root |
| `cbrt(x)` | Cube root |

### Rounding

| Function | Description |
|----------|-------------|
| `round(x)` | Round to nearest integer |
| `round(x, n)` | Round to n decimal places |
| `floor(x)` | Round down |
| `ceil(x)` | Round up |
| `trunc(x)` | Truncate toward zero |
| `abs(x)` | Absolute value |

### Aggregate functions (over ranges)

| Function | Description |
|----------|-------------|
| `sum(A1:A10)` | Sum of a range |
| `min(A1:A10)` | Minimum value |
| `max(A1:A10)` | Maximum value |
| `count(A1:A10)` | Count of non-empty cells |
| `average(A1:A10)` | Arithmetic mean |
| `stddev(A1:A10)` | Standard deviation |

### Conditional

| Function | Description | Example |
|----------|-------------|---------|
| `if(cond, a, b)` | Return `a` if cond is true, else `b` | `=if(A1 > 10, 1, 0)` |

---

## Constants

| Constant | Value |
|----------|-------|
| `pi` | 3.14159265… |
| `e` | 2.71828182… |
| `True` / `False` | Boolean literals |

---

## String and text functions

| Function | Description |
|----------|-------------|
| `str(x)` | Convert number to string |
| `upper(s)` | Uppercase string |
| `lower(s)` | Lowercase string |
| `len(s)` | String length |
| `concat(a, b)` | Concatenate strings |

> **Note:** Text functions are useful for building label strings in the
> spreadsheet but the result cannot be used as a dimension value.

---

## Deep dive: unit arithmetic

#### Why units matter

A common mistake is to enter `200` (bare number) in a cell and then use it
as a dimension. FreeCAD stores bare numbers as unit-less and may treat them
as millimetres or internal units depending on context. Always write `200 mm`,
`3.14 deg`, etc. to be explicit.

#### Mixing units

```
= 100 mm + 5 cm          # 150 mm ✓
= 100 mm + 5 in          # 227.0 mm ✓ (5 in = 127 mm)
= 100 mm * 50 mm         # 5000 mm² (area — the unit squares)
= 1 kg * 9.81 m/s^2      # 9.81 N (force = mass × acceleration)
```

#### Extracting a value without units

Some operations require a unit-less number. Use division:

```
= (200 mm) / mm          # 200 (unit-less) — rarely needed
```

---

## Common mistakes and pitfalls

!!! warning "Circular references are forbidden"
    If cell A1 references B1 and B1 references A1, FreeCAD reports a circular
    dependency error and refuses to recompute. Design your formula graph to be
    a DAG (directed acyclic graph) — no cycles.

!!! warning "Bare numbers vs unit quantities"
    `200` and `200 mm` behave differently in dimension fields. Use unit
    quantities in cells that feed into dimensional parameters.

!!! warning "Case-sensitive function names"
    `sin` is valid; `Sin` and `SIN` are not. Keep function names lowercase.

!!! warning "Comma vs semicolon as argument separator"
    Depending on your locale settings, FreeCAD may expect `;` as the function
    argument separator instead of `,`. If `if(A1>0, 1, 0)` fails, try
    `if(A1>0; 1; 0)`.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
sheet = doc.addObject("Spreadsheet::Sheet", "Sheet")

# Set formulas
sheet.set("A1", "50 mm")
sheet.set("A2", "30 mm")
sheet.set("A3", "=A1 + A2")           # 80 mm
sheet.set("A4", "=sqrt(A1^2 + A2^2)") # hypotenuse: ~58.3 mm
sheet.set("A5", "=if(A1 > A2, A1, A2)") # max: 50 mm

doc.recompute()

print(sheet.get("A3"))   # 80.0
print(sheet.get("A4"))   # ~58.3
print(sheet.get("A5"))   # 50.0

# Read a cell's expression (formula text)
print(sheet.getContents("A3"))   # "=A1 + A2"
```

---

## See also

- [Aliases](aliases.md) — name cells so expressions read as `Spreadsheet.Width` not `Spreadsheet.B3`
- [Model Integration](model-integration.md) — use expressions to read model properties and drive dimensions
- [Create Spreadsheet](create-spreadsheet.md) — how to create a spreadsheet to write formulas in
