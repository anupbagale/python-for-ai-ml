# Module 12 Cheatsheet

## The setup

Features `X`, target `y`, model `f`, loss `L(y, ŷ)`. Training makes the loss small on the training data; the goal is a small loss on **new** data, estimated with held-out data or cross-validation.

## Linear regression

```python
A = np.column_stack([X, np.ones(len(X))])
coef = np.linalg.lstsq(A, y, rcond=None)[0]        # weights, then bias; safer than the normal equations
LinearRegression().fit(X, y)                         # .coef_, .intercept_
```

A weight is the effect of its feature **with the others held fixed**. Strongly correlated features (multicollinearity) make weights unstable: check with VIF = 1 / (1 - R²ⱼ), and treat VIF above 5 to 10 as a warning.

## Regression metrics

| metric | formula | use |
|---|---|---|
| MAE | `mean(abs(e))` | typical error, in target units |
| RMSE | `sqrt(mean(e**2))` | punishes big misses |
| R² | `1 - SSE / SST` | compared with predicting the mean; 0 = no better, negative = worse |
| MAPE | `mean(abs(e / y))` | percent error; breaks near y = 0 |

Always compare with the baseline (predict the mean) and look at **residual plots**: a curve means a missing non-linear effect; a widening band (heteroscedasticity) suggests a log target.

```python
from sklearn.model_selection import cross_val_predict
pred = cross_val_predict(model, X, y, cv=cv)          # honest predictions for every row
```

## Features

```python
PolynomialFeatures(degree=2, include_bias=False)      # squares and interactions
PolynomialFeatures(2, interaction_only=True)          # products only
SplineTransformer(n_knots=8, degree=3)                # smooth curves, calm at the edges
FunctionTransformer(np.log, feature_names_out="one-to-one")
TransformedTargetRegressor(model, func=np.log, inverse_func=np.exp)   # log target, predictions in original units
```

Logs for skewed positive values, ratios for totals, splines for curves, interactions for effects that depend on each other.

## Bias and variance

`expected error = bias² + variance + noise`

- **bias**: wrong the same way every time (too rigid)
- **variance**: wrong differently for each training set (too flexible)
- more flexibility: bias down, variance up; more data: variance down

## Regularisation

```python
make_pipeline(StandardScaler(), Ridge(alpha=1.0))     # L2: shrinks smoothly, fixes multicollinearity
make_pipeline(StandardScaler(), Lasso(alpha=0.1))     # L1: exact zeros, feature selection
ElasticNet(alpha=0.1, l1_ratio=0.5)                    # both
RidgeCV(alphas=np.logspace(-3, 3, 13)); LassoCV(cv=5)  # choose alpha by cross-validation
```

Ridge closed form, on centred data: `w = solve(XcᵀXc + αI, Xcᵀyc)`, `b = mean(y) - mean(X)·w`.

Lasso soft-thresholding: `w_j = sign(ρ) · max(|ρ| - α, 0) / z_j`.

- **Standardise first** (the penalty treats all weights alike), but not features already on a 0 to 1 scale, like spline bases.
- Don't penalise the intercept.
- A penalty stops useless features hurting; it can't make them useful.

## Learning curves

```python
from sklearn.model_selection import learning_curve, validation_curve
sizes, train, val = learning_curve(model, X, y, train_sizes=np.linspace(0.1, 1, 8), cv=cv,
                                   scoring="neg_root_mean_squared_error")
```

| shape | diagnosis | try |
|---|---|---|
| both high, close together | high bias | more features, more flexible model, less regularisation |
| low training, big gap, gap shrinking | high variance | more data, more regularisation, simpler model |
| both low, close | good | test it |

## Logistic regression

```python
LogisticRegression(C=1.0)        # C is the INVERSE penalty: small C = strong regularisation
```

- Binary boundary: the line `w·x + b = 0`; probability 0.5 there.
- More than two classes: softmax over one score per class; gradient `P - Y`.
- Curved boundaries need features (polynomials, splines).
- scikit-learn's penalty: `l2 = 1 / (C · n)` in "mean loss + (l2 / 2)·‖W‖²" terms.

## Validation that matches use

- Random folds: new rows from familiar places or groups.
- `GroupKFold`: new places, people or groups.
- Time-based splits: the future.

Different schemes can give different errors **and** different best hyperparameters.

## Target encoding without leakage

Encode each row from **other folds only** (out of fold), smooth rare categories towards the mean, and use the full-data mapping for new data. In-sample encoding fakes signal, even on pure noise.
