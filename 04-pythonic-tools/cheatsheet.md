# Module 4 Cheatsheet

## Iteration

```python
it = iter(iterable)     # get an iterator
next(it)                # next item, StopIteration at the end
next(it, default)       # default instead of StopIteration
```

Iterators are single-use. `zip`, `map`, `filter`, `enumerate`, `reversed` and file objects return iterators: use `list(...)` if you need to go over them twice.

## Generators

```python
def countdown(n):
    while n > 0:
        yield n          # hand out a value, pause here
        n -= 1

gen = countdown(3)       # nothing runs yet
list(gen)                # [3, 2, 1]; gen is now exhausted

squares = (x * x for x in data)      # generator expression
yield from other_iterable            # delegate to another iterable
return                               # ends the generator
```

Memory use doesn't grow with the number of items. Generators can be infinite.

Don't yield a mutable object you keep modifying (yield `tuple(window)`, not `window`).

## Pipelines

```python
records = parse(read_lines(path))
records = only_valid(records)
for batch in batched(records, 32):
    train_step(batch)
```

## itertools

```python
from itertools import islice, count, cycle, chain, product, combinations, accumulate, groupby

islice(it, 5)                      # first 5 items, lazily
islice(it, 10, 20)                 # items 10..19
count(1), cycle(xs)                # infinite
chain(a, b), chain.from_iterable(nested)
product(*grid.values())            # every combination
[dict(zip(grid, v)) for v in product(*grid.values())]   # hyperparameter grid
combinations(xs, 2)                # every pair
accumulate(xs), accumulate(xs, min)
groupby(sorted(xs, key=k), key=k)  # only groups CONSECUTIVE items: sort first
batched(xs, 3)                     # Python 3.12+, yields tuples

batch = list(islice(it, size))     # next `size` items from the same iterator
```

## Decorators

```python
import functools

def timed(func):
    @functools.wraps(func)                 # keep name and docstring
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(func.__name__, time.perf_counter() - start)
        return result
    return wrapper

@timed                                     # same as f = timed(f)
def f(): ...

def retry(times):                          # decorator with arguments: one more layer
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs): ...
        return wrapper
    return decorator

REGISTRY = {}
def register(name):                        # registry: record and return unchanged
    def decorator(obj):
        REGISTRY[name] = obj
        return obj
    return decorator
```

Attach state to the wrapper with `wrapper.calls = 0`. Use `nonlocal` to assign to an enclosing variable.

## functools

```python
@functools.cache                  # unlimited memoization
@functools.lru_cache(maxsize=128) # bounded
f.cache_info()

functools.partial(f, weight=10)   # pre-fill arguments
```

Only cache pure functions with hashable arguments. Return immutable values (tuples) from cached functions.

## Context managers

```python
class Timer:
    def __enter__(self):
        self.start = time.perf_counter()
        return self                         # what `as` gets
    def __exit__(self, exc_type, exc, tb):
        self.elapsed = time.perf_counter() - self.start
        return False                        # True would swallow the exception

from contextlib import contextmanager

@contextmanager
def temporary_seed(seed):
    state = random.getstate()
    random.seed(seed)
    try:
        yield                               # the with-block runs here
    finally:
        random.setstate(state)              # always runs
```

Save the previous value and restore it (rather than resetting to a default) so nesting works. Use a sentinel (`_MISSING = object()`) when `None` is a valid value.

## Type hints

```python
from typing import Callable, Iterator, Protocol

def f(xs: list[float], k: int = 3) -> dict[str, float]: ...
def g(path: str, limit: int | None = None) -> list[str]: ...
def h(metric: Callable[[list[int], list[int]], float]) -> float: ...
def gen(n: int) -> Iterator[list[int]]: ...

Batch = list[tuple[float, float]]           # type alias

class Classifier(Protocol):                 # duck-typed interface
    def fit(self, X, y) -> "Classifier": ...
    def predict(self, X) -> list[int]: ...
```

Not checked at runtime. Use `mypy` to check them.

## Common bugs from this module

| Bug | Fix |
|---|---|
| looping over a `zip`/`map` twice gives nothing | `list(...)` it first |
| `list()` on an infinite generator hangs | use `islice` |
| batching with `islice(the_list, n)` repeats the first batch | call `iter()` once, slice that |
| generator yields the same mutable object | yield a copy or tuple |
| decorated function shows up as `wrapper` | `@functools.wraps(func)` |
| registry decorator forgets `return obj` | name becomes `None` |
| `groupby` splits a group in two | sort by the key first |
| context manager leaves state changed after an error | `try/finally` around `yield` |
| `except Exception` around the whole loop body hides bugs | catch the specific error, around the specific line |
| evaluating on the test set more than once | choose with validation, test once at the end |
