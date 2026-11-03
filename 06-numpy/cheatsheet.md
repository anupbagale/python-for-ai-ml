# Module 6 Cheatsheet

## Creating arrays

```python
import numpy as np

np.array([[1, 2], [3, 4]])        # dtype inferred: one stray string makes everything text
np.zeros((2, 3)), np.ones(3), np.full((2, 2), 7.0), np.eye(3)
np.arange(0, 10, 2)               # integer steps
np.linspace(0, 1, 5)              # exact number of points, end included
a.astype(np.float32)
a.tolist(), float(a.sum())        # back to Python types (for JSON)
```

## Shape

```python
a.shape, a.ndim, a.size, a.dtype
a.reshape(3, -1)                  # -1 = work it out
v[:, None]                        # (n,) -> (n, 1)
v[None, :]                        # (n,) -> (1, n)
a.T                               # transpose
np.column_stack([x, y]), np.vstack([...]), np.concatenate([...])
```

Datasets are `(n_samples, n_features)`.

## Indexing

```python
X[1, 2]          # row 1, column 2
X[:, 0]          # first column
X[1:, ::2]
X[mask]          # boolean mask -> copy
X[[4, 0, 2]]     # fancy indexing -> copy
X[np.arange(n), y]                # one element per row (e.g. the correct class)
np.where(cond, a, b)
np.flatnonzero(cond)              # positions where cond is True
(a > 0) & (b < 1), ~mask          # not `and`/`not`; parenthesise comparisons
```

**Basic slices are views:** they share memory with the original. Use `.copy()` when you need an independent array. Check with `np.shares_memory(a, b)`.

## Reductions

```python
X.sum(), X.mean(axis=0), X.std(axis=0), X.max(axis=1), X.argmax(axis=1)
X.sum(axis=1, keepdims=True)      # (n, 1) instead of (n,)
np.nanmean(x)                     # ignore NaN
(pred == y).mean()                # accuracy
```

`axis` is the dimension that disappears. `axis=0` gives one value per column (feature).

## Broadcasting

Compare shapes from the right. Dimensions must be equal, or one of them 1.

```python
(X - X.mean(axis=0)) / X.std(axis=0)          # (n, d) with (d,)
X - X.mean(axis=1, keepdims=True)             # (n, d) with (n, 1)
A[:, None, :] - B[None, :, :]                 # (n, 1, d) - (1, m, d) -> (n, m, d)
```

When it fails, print both shapes and line them up from the right.

## Linear algebra

```python
A @ B                     # (n, k) @ (k, m) -> (n, m)
X @ W + b                 # a linear layer for a whole batch
A * B                     # element-wise, NOT matrix multiplication
np.linalg.solve(A, b)     # never inv(A) @ b
np.linalg.lstsq(X, y, rcond=None)
np.linalg.norm(v), np.linalg.norm(X, axis=1)
```

## Random

```python
rng = np.random.default_rng(seed)
rng.random(3), rng.normal(0, 1, size=(2, 3)), rng.integers(0, 10, size=5)
rng.choice(items, size=4), rng.permutation(n)

idx = rng.permutation(len(X))      # shuffle X and y TOGETHER
X, y = X[idx], y[idx]
```

## Counting and accumulating

```python
np.bincount(labels, minlength=k)                        # counts per class
np.bincount(t * k + p, minlength=k * k).reshape(k, k)   # confusion matrix
np.add.at(M, (rows, cols), 1)                           # += that handles repeated indexes
```

`M[rows, cols] += 1` counts a repeated pair only once.

## Numerically stable formulas

```python
z = z - z.max(axis=-1, keepdims=True)                    # before exp
softmax = np.exp(z) / np.exp(z).sum(axis=-1, keepdims=True)
log_softmax = z - np.log(np.exp(z).sum(axis=-1, keepdims=True))
np.sqrt(np.maximum(sq_dist, 0))                          # rounding can make it slightly negative
```

## Pitfalls

| Pitfall | Fix |
|---|---|
| `uint8` image + 100 wraps around | `img.astype(np.float32) / 255` first |
| float32 loses tiny values | know which precision you need |
| one NaN ruins `mean()` | `np.nanmean`, or clean first |
| `a == b` on floats | `np.isclose`, `np.allclose` |
| `X_train = X[:800]` then scaling in place changes `X` | `.copy()` |
| `np.eye(k)[-1]` silently picks the last row | validate labels |
| in-place `z -= ...` in a function changes the caller's array | `z = z - ...` |
| Python `for` over array elements | vectorize |

## Neural network shapes (one hidden layer)

| | forward | backward |
|---|---|---|
| layer 1 | `Z1 = X @ W1 + b1` | `dW1 = X.T @ dZ1`, `db1 = dZ1.sum(0)` |
| ReLU | `A1 = np.maximum(Z1, 0)` | `dZ1 = dA1 * (Z1 > 0)` |
| layer 2 | `Z2 = A1 @ W2 + b2` | `dW2 = A1.T @ dZ2`, `dA1 = dZ2 @ W2.T` |
| loss | cross-entropy on `Z2` | `dZ2 = (softmax(Z2) - one_hot(y)) / n` |

A gradient always has the same shape as the thing it's the gradient of. Always run a numerical gradient check.

## Saving arrays

```python
np.savez_compressed("model.npz", **params)
with np.load("model.npz") as f:           # allow_pickle=False by default: keep it that way
    params = {k: f[k] for k in f.files}
```
