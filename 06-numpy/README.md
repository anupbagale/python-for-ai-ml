# Module 6: NumPy

Arrays instead of loops. This module shows why NumPy is fast, then builds up to vectorized versions of the algorithms written by hand in earlier modules, and ends with a neural network in NumPy that reads handwritten digits.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | From "why is it fast" to k-NN with no loops | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/01_lesson.ipynb) |
| 2 | Exercises | 12 vectorized building blocks of ML code, with array-aware checks | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, following the shapes step by step | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/03_solutions.ipynb) |
| 4 | Mini project | A neural network in NumPy that reads handwritten digits | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/06-numpy/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Why NumPy is fast.** A measured comparison with plain Python, and what contiguous, single-type memory has to do with it.

**Creating arrays.** dtypes, and the stray string that turns a whole array into text. `zeros`, `ones`, `eye`, `arange` vs `linspace`.

**Shape.** `(n_samples, n_features)`, `reshape(-1, ...)`, `[:, None]`, `.T`, and `(3,)` vs `(3, 1)`.

**Indexing.** Slicing, the view trap (slices share memory with the original), boolean masks, `np.where`, and fancy indexing.

**Vectorized operations.** ufuncs, reductions, and the rule for `axis`: it's the dimension that disappears.

**Broadcasting.** The rules, standardizing a whole dataset in one line, a common shape error and how to read it, and building pairwise tables.

**Numerical pitfalls.** Integer overflow in image data, float32 vs float64, NaN spreading, and comparing floats.

**Linear algebra.** `@` and its shape rule, a linear layer for a whole batch, and solving linear regression exactly with `solve` and `lstsq`.

**Random numbers.** `default_rng`, and shuffling `X` and `y` together safely.

**Putting it together.** k-NN with no loops, giving identical predictions to the Module 1 version, many times faster.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `standardize`, `one_hot`, `softmax`, `split_indices` |
| Medium | `pairwise_distances`, `knn_predict`, `confusion_matrix`, `moving_average`, `minibatches` |
| Hard | `ridge_fit`, `cross_entropy_with_grad`, `conv2d` |

The checks target the traps that make NumPy code silently wrong: modifying the caller's array, negative labels that index from the end, `M[t, p] += 1` ignoring repeated pairs, square roots of tiny negative numbers, and shuffling `X` and `y` separately.

## Mini project

A one-hidden-layer neural network on 1,797 handwritten digits (the UCI digits dataset bundled with scikit-learn). The backward pass is derived from a table of shapes, then verified with a numerical gradient check, which also catches a deliberately planted bug. Training uses mini-batches and early stopping on a validation set, and takes a fraction of a second. The network reaches 97.4% test accuracy, just behind a k-NN baseline at 98.5%, and the notebook explains why simple methods do so well on small, clean images. It finishes with a confusion matrix, a look at the mistakes (most are predicted as "1"), and saving and loading the weights safely with `np.savez`.

## Prerequisites

Modules [1](../01-python-foundations) to [5](../05-files-and-data). The mini project builds directly on the [Module 3](../03-oop-for-ml) autograd project.

## Next

[Module 7: pandas](../07-pandas)
