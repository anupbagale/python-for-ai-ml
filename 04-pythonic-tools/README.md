# Module 4: Pythonic Tools

Iterators, generators, `itertools`, decorators, context managers and type hints: the features behind data loaders, `@torch.no_grad()`, model registries, caching and clean, reusable ML code.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | From the iterator protocol to a streaming training pipeline | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/01_lesson.ipynb) |
| 2 | Exercises | 12 generators, decorators and context managers with scenario-based checks | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions with notes on the subtle parts | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/03_solutions.ipynb) |
| 4 | Mini project | A hyperparameter search framework | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/04-pythonic-tools/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Iteration.** What a `for` loop really does, iterables vs iterators, and why `zip`, `map` and files can only be looped over once.

**Generators.** `yield`, lazy evaluation, constant memory use, infinite generators, `yield from` and generator expressions.

**Generator pipelines.** Read, parse, filter and batch a million-line stream while only ever holding a few items in memory.

**itertools.** `islice`, `count`, `cycle`, `chain`, `product` for hyperparameter grids, `combinations`, `accumulate`, `groupby` (and why it needs sorted data) and `batched`.

**Decorators.** How they work, `functools.wraps`, decorators with arguments (`@retry(times=3)`), and registry decorators like the ones in timm and Hugging Face.

**functools.** `lru_cache` and `cache` for memoization, and `partial`.

**Context managers.** `__enter__`/`__exit__`, `@contextmanager` with `try/finally`, temporarily fixing a random seed, and a `no_grad()` that works the way PyTorch's does.

**Type hints.** Annotating functions, `X | None`, `Callable`, `Iterator`, and `Protocol` for duck-typed interfaces.

**Putting it together.** A streaming training pipeline over an infinite data source, with corrupted rows filtered on the way, a timing decorator and a timing context manager.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `take`, `running_total`, `count_calls`, `Timer` |
| Medium | `batched`, `sliding_window`, `parse_scores`, `retry`, `memoize` |
| Hard | `Registry`, `interleave`, `temporary_attrs` |

The checks test the tricky parts directly: that generators work on infinite input, that iterators aren't over-consumed, that decorated functions keep their names, and that context managers restore state and don't swallow exceptions.

## Mini project

A small hyperparameter search tool, in the spirit of `GridSearchCV` or Optuna: a model registry, a search space expanded lazily with `itertools.product`, a cached data loader, a context manager that records failing trials instead of crashing, and a generator that streams results live. It compares a baseline, k-NN and logistic regression. Then it shows how unscaled features make logistic regression both worse and very sensitive to its settings (49% to 78%), adds a scaled version through the registry without touching the search code (a stable 87% to 88%), and finally evaluates the winner on the test set once.

## Prerequisites

Modules [1](../01-python-foundations), [2](../02-data-structures) and [3](../03-oop-for-ml)

## Next

[Module 5: Files and Data](../05-files-and-data)
