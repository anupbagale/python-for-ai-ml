# Module 1: Python Foundations

The core of the language, taught with ML work in mind. Pure Python only, no libraries.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Concepts and runnable examples, ending with gradient descent in pure Python | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/01_lesson.ipynb) |
| 2 | Exercises | 12 problems (Easy, Medium, Hard) with automatic checks | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions with notes on each approach | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/03_solutions.ipynb) |
| 4 | Mini project | A k-nearest neighbours classifier built from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/01-python-foundations/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Variables and names.** How assignment works, why `b = a` doesn't copy a list, and the difference between `==` and `is`.

**Numbers.** Integer vs float division, why `0.1 + 0.2 != 0.3`, how to compare floats safely, and how to deal with `inf` and `nan` when training goes wrong.

**Strings.** Slicing, the string methods you need for cleaning text, and f-string formatting for readable training logs.

**Booleans and None.** Chained comparisons, truthiness, and why `if value:` is a bug when zero is a valid value.

**Control flow.** `if`/`elif`/`else`, `for` with `range`, `enumerate` and `zip`, `while`, `break` and `continue`, with an early stopping example.

**Functions.** Default and keyword arguments, returning multiple values, the mutable default argument trap, `*args` and `**kwargs`, scope, passing functions as arguments, and `lambda`.

**Comprehensions.** List, dict and set comprehensions, filtering vs conditional expressions, nested comprehensions, and generator expressions.

**Errors.** Catching specific exceptions and raising clear errors on bad input.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `accuracy`, `clip`, `is_valid_probability`, `count_labels` |
| Medium | `normalize`, `mean_and_std`, `one_hot`, `train_test_split`, `confusion_counts` |
| Hard | `precision_recall_f1`, `moving_average`, `batch` |

Every exercise is a small piece of real ML tooling. By the end you'll have written your own versions of functions that scikit-learn and PyTorch provide.

## Mini project

Classify fruit as apple, orange or lemon from weight and sweetness using k-NN, written from scratch. The project walks through shuffling and splitting data, measuring distance, voting, evaluating different values of k, and then shows how forgetting to scale features drops accuracy from about 95% to as low as 70%.

## Prerequisites

None. If you've never written Python, it helps to spend an hour on the basics of running code first, but the lesson starts from the beginning.

## Next

[Module 2: Data Structures](../02-data-structures)
