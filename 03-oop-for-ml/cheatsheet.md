# Module 3 Cheatsheet

## Class basics

```python
class StandardScaler:
    def __init__(self):            # runs on StandardScaler()
        self.mean_ = None          # instance attribute

    def fit(self, values):         # method; self is the instance
        self.mean_ = sum(values) / len(values)
        return self                # allows chaining

scaler = StandardScaler().fit(train)
scaler.fit(x)  ==  StandardScaler.fit(scaler, x)
```

Conventions: `CapitalizedNames` for classes, `learned_` attributes end in `_`, `_internal` attributes start with `_`.

## Class vs instance attributes

```python
class Experiment:
    framework = "torch"        # shared by all instances: fine for constants
    history = []               # BUG: shared mutable list

    def __init__(self):
        self.history = []      # correct: one list per instance
```

## Dunder methods

```python
def __repr__(self):       return f"Model(w={self.w})"   # print(obj), debugging
def __call__(self, x):    return self.w * x            # obj(x)
def __len__(self):        return len(self.items)       # len(obj)
def __getitem__(self, i): return self.items[i]         # obj[i], also enables for loops
def __iter__(self):       yield from self.items        # for x in obj
def __contains__(self, x):return x in self.items       # x in obj
def __eq__(self, other):                               # obj == other
    if not isinstance(other, type(self)):
        return NotImplemented
    return self.w == other.w
def __add__(self, other): ...                          # obj + other
def __radd__(self, other):return self + other          # 0 + obj, needed for sum()
def __mul__(self, k): ...                              # obj * 3
def __rmul__(self, k):    return self * k              # 3 * obj
```

Defining `__eq__` without `__hash__` makes instances unhashable (can't be set members or dict keys).

## Properties

```python
class Classifier:
    @property
    def threshold(self):
        return self._threshold

    @threshold.setter
    def threshold(self, value):
        if not 0 <= value <= 1:
            raise ValueError("threshold must be in [0, 1]")
        self._threshold = value
```

## Inheritance and ABCs

```python
from abc import ABC, abstractmethod

class BaseClassifier(ABC):
    @abstractmethod
    def fit(self, X, y): ...

    @abstractmethod
    def predict(self, X): ...

    def score(self, X, y):                         # shared by all subclasses
        preds = self.predict(X)
        return sum(p == t for p, t in zip(preds, y)) / len(y)

class Majority(BaseClassifier):
    def __init__(self, name):
        super().__init__()                         # always call the parent's __init__
        ...

isinstance(obj, BaseClassifier)
type(self).__name__                                # actual class name, for __repr__
```

## Composition

```python
class VotingClassifier(BaseClassifier):
    def __init__(self, models):
        self.models = models        # has-a, not is-a
```

Use inheritance for "is a kind of" with a shared interface. Use composition for everything else.

## Dataclasses

```python
from dataclasses import dataclass, field, asdict

@dataclass
class TrainConfig:
    lr: float = 1e-3
    hidden: list = field(default_factory=lambda: [64, 32])   # never = [64, 32]

    def __post_init__(self):                                 # validation
        if self.lr <= 0:
            raise ValueError("lr must be positive")

@dataclass(frozen=True)       # read-only, hashable
class Result:
    run_id: str
    acc: float

asdict(config)                # to a dict (for JSON)
```

Type hints in dataclasses are not enforced at runtime.

## Class and static methods

```python
class Config:
    @classmethod
    def from_dict(cls, d):        # alternative constructor
        return cls(**d)

    @staticmethod
    def helper(x):                # no self, no cls
        return x * 2
```

## Dataset / DataLoader pattern

```python
class MyDataset:
    def __len__(self): ...
    def __getitem__(self, i): return x_i, y_i

class DataLoader:
    def __iter__(self):
        idx = list(range(len(self.dataset)))
        if self.shuffle: self._rng.shuffle(idx)
        for s in range(0, len(idx), self.batch_size):
            batch = [self.dataset[i] for i in idx[s:s + self.batch_size]]
            xs, ys = zip(*batch)
            yield list(xs), list(ys)
```

## Training loop shape

```python
for epoch in range(epochs):
    for xb, yb in loader:
        loss = loss_fn(model(xb), yb)
        optimizer.zero_grad()      # gradients accumulate otherwise
        loss.backward()
        optimizer.step()
```

## Common bugs from this module

| Bug | Fix |
|---|---|
| mutable class attribute shared by instances | create it in `__init__` |
| forgot `super().__init__()` | call it first in the subclass `__init__` |
| `return self.items` from a property exposes internal state | return a copy |
| storing the caller's list directly | `self.values = list(values)` |
| `10 * obj` raises TypeError | add `__rmul__` |
| `sum(objects)` fails | add `__radd__` (sum starts from 0) |
| averaging per-batch metrics | accumulate counts, compute at the end |
| gradients doubling every step | `zero_grad()` before `backward()` |
