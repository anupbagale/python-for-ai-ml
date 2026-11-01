# Module 1 Cheatsheet

## Names and copies

```python
b = a            # same object, not a copy
b = a.copy()     # independent shallow copy (also list(a), a[:])
a is b           # same object?
a == b           # same value?
```

## Numbers

```python
7 / 2     # 3.5   true division
7 // 2    # 3     floor division
7 % 2     # 1     remainder
2 ** 10   # 1024  power

int(3.99)    # 3   truncates toward zero
round(2.5)   # 2   rounds halves to even

import math
math.isclose(0.1 + 0.2, 0.3)   # compare floats like this, never with ==
math.isnan(x)                  # nan == nan is False, so use this
float("inf"), float("nan")
```

## Strings

```python
s[start:stop:step]     # stop is excluded
s[::-1]                # reversed
s.strip().lower().split()
" ".join(words)
s.replace("a", "b")
s.startswith("x")
```

## f-strings

```python
f"{loss:.4f}"     # 0.0457
f"{acc:.2%}"      # 91.23%
f"{lr:.1e}"       # 3.0e-04
f"{n:,}"          # 1,234,567
f"{x:>8}"         # right-align, width 8
f"{epoch:03d}"    # 007
f"{lr=}"          # lr=0.0003
```

## Booleans and None

```python
0 <= p <= 1            # chained comparison
x is None              # not x == None
if x is not None:      # use this when 0 or "" are valid values

# falsy: 0, 0.0, "", [], {}, set(), None
```

## Loops

```python
for i in range(start, stop, step): ...
for i, item in enumerate(items, start=1): ...
for a, b in zip(xs, ys): ...
for a, b in zip(xs, ys, strict=True): ...   # error on length mismatch (3.10+)

while condition: ...
break       # leave the loop
continue    # skip to the next iteration
```

## Functions

```python
def f(x, threshold=0.5):     # default argument
    """Docstring."""
    return a, b              # returns a tuple

lo, hi = f(x)                # unpacking

def f(items=None):           # never use [] or {} as a default
    if items is None:
        items = []

def f(*args, **kwargs): ...  # args is a tuple, kwargs is a dict
f(*my_list, **my_dict)       # unpack into arguments

max(results, key=lambda r: r[1])
sorted(results, key=lambda r: r[1], reverse=True)
```

## Comprehensions

```python
[x ** 2 for x in xs]                  # transform
[x for x in xs if x > 0]              # filter: if at the end
[1 if p >= 0.5 else 0 for p in ps]    # choose: if/else at the front
{k: v for k, v in pairs}              # dict
{w for w in words}                    # set
[x for row in matrix for x in row]    # flatten
sum(x * x for x in xs)                # generator expression, no list built
```

## Errors

```python
try:
    risky()
except ValueError as e:     # catch specific exceptions only
    print(e)

if len(a) != len(b):
    raise ValueError(f"length mismatch: {len(a)} vs {len(b)}")
```

## Common ML gotchas from this module

| Gotcha | Fix |
|--------|-----|
| `loss == best_loss` with floats | compare with a tolerance |
| `nan` loss breaks comparisons | check with `math.isnan` |
| `if dropout:` when dropout can be 0 | `if dropout is not None:` |
| `def f(history=[])` | default to `None` |
| `zip` silently drops items | `zip(..., strict=True)` |
| `data[:-n]` when n is 0 | compute the split index from the start |
| `int(len(data) * ratio)` | `round(...)` |
| scaling test data with its own min/max | fit on train, apply to test |
