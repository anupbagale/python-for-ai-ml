# Module 9 Cheatsheet

## The habit

1. Write the formula.
2. Implement it.
3. **Check it** against something independent: an exact answer, a numerical approximation, a simulation, or a second method.

## Floating point

```python
np.isclose(0.1 + 0.2, 0.3)        # never compare floats with ==
np.finfo(float).eps               # about 2.2e-16
np.var(x)                         # not mean(x**2) - mean(x)**2 (catastrophic cancellation)
```

| problem | fix |
|---|---|
| `exp` overflows above about 709 | subtract the max first |
| `log(0)` is `-inf` | clip: `np.clip(p, 1e-12, 1)`, or work in log space |
| subtracting nearly equal large numbers | rearrange the formula (centre the data first) |

```python
def softmax(z):                                   # along the last axis
    e = np.exp(z - z.max(axis=-1, keepdims=True))
    return e / e.sum(axis=-1, keepdims=True)

def log_sum_exp(z):
    c = z.max()
    return c + np.log(np.sum(np.exp(z - c)))
```

## Vectors and matrices

```python
a @ b                                   # dot product
np.linalg.norm(a)                       # length
a @ b / (norm(a) * norm(b))             # cosine similarity (-1 to 1)
X @ w                                   # (n, d) @ (d,) -> (n,): one prediction per row
X.T @ error                             # (d, n) @ (n,) -> (d,): one gradient entry per weight
np.linalg.solve(A, b)                   # solve A x = b (not inv(A) @ b)
np.linalg.lstsq(A, b, rcond=None)[0]    # least squares
```

## Derivatives and gradients

```python
(f(x + h) - f(x - h)) / (2 * h)         # central difference, error ~ h^2; h = 1e-5
```

```python
def numerical_gradient(f, x, h=1e-5):
    x = np.array(x, dtype=float)        # float copy
    grad = np.zeros_like(x)
    for i in np.ndindex(x.shape):
        orig = x[i]
        x[i] = orig + h; fp = f(x)
        x[i] = orig - h; fm = f(x)
        x[i] = orig
        grad[i] = (fp - fm) / (2 * h)
    return grad

rel_err = norm(g1 - g2) / (norm(g1) + norm(g2))   # < 1e-7 good, > 1e-3 bug
```

Check at a **random** point, and confirm the check fails on a planted bug.

## Chain rule

`d/dx f(g(x)) = f'(g(x)) · g'(x)`. Backprop multiplies local derivatives from the output backwards. Gradients **accumulate** (`+=`) when a value is used more than once.

| function | derivative |
|---|---|
| `a + b` | 1 for each |
| `a * b` | `b` for `a`, `a` for `b` |
| `tanh(v)` | `1 - tanh(v)²` |
| `sigmoid(z)` | `s(1 - s)` |
| `log(1 + e^z) - y·z` (log loss) | `sigmoid(z) - y` |
| `mean((Xw + b - y)²)` | `2/n · Xᵀe` and `2 · mean(e)` |

## Gradient descent

```python
x = x - lr * grad(x)                    # not x -= ... if you keep a history
```

- Steepest direction sets the largest safe `lr` (for `c·x²`, need `lr < 1/c`)
- Shallowest direction sets how many steps it takes
- **Standardise features** to make the bowl round
- Too small: slow. Too big: oscillates or diverges.

Newton's method: `θ ← θ - solve(H, ∇L)`. Few steps, but each needs a `d × d` Hessian.

## Logistic regression

```python
z = X @ w + b
loss = np.mean(np.logaddexp(0, z) - y * z)        # stable log loss
p = sigmoid(z)                                     # stable: two formulas by sign of z
grad_w = X.T @ (p - y) / n
grad_b = np.mean(p - y)
hessian = A.T @ (A * (p * (1 - p))[:, None]) / n   # A = X with a column of ones
```

Starting loss with zero weights: `log(2)` (binary), `log(k)` (k classes).

## Probability

```python
rng = np.random.default_rng(42)
rng.random(n) < p                       # n Bernoulli(p) draws
rng.integers(1, 7, size=n)              # dice
rng.normal(mu, sigma, size=n)
rng.choice([-1, 1], size=n)
```

Bayes: `P(A|B) = P(B|A) P(A) / P(B)`. Positive test: `TP / (TP + FP)` where `TP = sens · prior`, `FP = (1 - spec)(1 - prior)`. **Mind the base rate.**

## Statistics

| thing | code |
|---|---|
| sample variance | `np.var(x, ddof=1)` (NumPy defaults to `ddof=0`, pandas to `ddof=1`) |
| standard error of a mean | `x.std(ddof=1) / np.sqrt(len(x))` |
| bootstrap resample | `x[rng.integers(0, n, size=n)]` |
| 95% percentile interval | `np.percentile(stats, [2.5, 97.5])` |
| permutation p-value | `(count + 1) / (n_perm + 1)`, with `abs()` for two-sided |
| paired comparison | sign-flip the per-example differences |

Uncertainty shrinks like `1 / sqrt(n)`: four times the data halves it. Report the **size** of an effect, not just the p-value.

## Information

```python
entropy = -np.sum(p * np.log(p))                    # uncertainty (skip p = 0)
cross_entropy = -np.sum(p * np.log(q))              # one-hot p: -log q[true class]
kl = cross_entropy - entropy                        # >= 0, zero only when q == p
probs[np.arange(n), labels]                         # true-class probability per row
```
