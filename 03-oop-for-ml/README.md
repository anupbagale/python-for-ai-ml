# Module 3: Object-Oriented Programming for ML

Classes taught through the patterns that scikit-learn and PyTorch actually use: `fit`/`transform`, `Dataset` and `DataLoader`, `nn.Module`, optimizers, metrics and configs.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Classes through to a PyTorch-style `Dataset` and `DataLoader` written from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/01_lesson.ipynb) |
| 2 | Exercises | 12 classes to write (Easy, Medium, Hard) with scenario-based checks | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions with notes on design decisions | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/03_solutions.ipynb) |
| 4 | Mini project | A tiny autograd engine, used to train a neural network from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/03-oop-for-ml/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Why classes.** When a class is the right tool (there's state to keep) and when a function is better.

**Classes and instances.** `__init__`, `self`, attributes and methods, built around a scikit-learn-style `StandardScaler`. Why `fit` returns `self`, and why learned attributes end in `_`.

**Class vs instance attributes.** The shared mutable class attribute bug, and how to avoid it.

**Dunder methods.** `__repr__`, `__call__` (how `model(x)` works in PyTorch), `__len__` and `__getitem__` (how datasets work), `__eq__`, and arithmetic operators including `__rmul__`.

**Properties.** Validating values like a classification threshold, and computed attributes that are always up to date.

**Inheritance and abstract base classes.** A `BaseClassifier` interface with `fit`, `predict` and `score`, a majority-class baseline, a decision stump, polymorphism, and `super().__init__()`.

**Composition.** A voting ensemble that holds other models, and an honest look at why it scored worse than its best member.

**Dataclasses.** Typed training configs with defaults, `field(default_factory=...)`, validation in `__post_init__`, and frozen results.

**Class and static methods.** Alternative constructors like `from_dict`, the pattern behind `from_pretrained`.

**Putting it together.** A `Dataset` base class, a `TabularDataset`, and a `DataLoader` with batching, shuffling and `drop_last`, plugged into a training loop.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `RunningMean`, `LabelEncoder`, `ExponentialMovingAverage`, `Subset` |
| Medium | `MinMaxScaler`, `EarlyStopping`, `Vector`, `TrainConfig`, `Accuracy` |
| Hard | `Pipeline`, `WarmupCosineSchedule`, `ConfusionMatrix` |

Each one is a small version of a class from scikit-learn, PyTorch or torchmetrics.

## Mini project

A `Value` class that records every calculation through operator overloading, and a `backward()` method that computes gradients with the chain rule. That's the core of PyTorch's autograd. On top of it: `Module`, `Neuron`, `Layer`, `MLP` and `SGD` classes, organised like PyTorch, and a training loop that teaches a small network to classify points inside a circle. It reaches about 98% test accuracy, and the gradients are checked against numerical estimates. The notebook ends with a side-by-side comparison with the equivalent PyTorch code.

## Prerequisites

[Module 1](../01-python-foundations) and [Module 2](../02-data-structures)

## Next

Module 4: Pythonic Tools (coming soon)
