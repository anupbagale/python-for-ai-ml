# Module 14 Cheatsheet

## Decision trees

```python
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor, export_text, plot_tree
tree = DecisionTreeClassifier(max_depth=4, min_samples_leaf=20, random_state=0).fit(X, y)
print(export_text(tree, feature_names=list(X.columns)))
tree.get_depth(), tree.get_n_leaves()
```

- Each internal node asks "is feature ≤ threshold?"; values equal to the threshold go **left**.
- A leaf predicts the majority class (classification) or the mean (regression).
- Boundaries are axis-aligned rectangles. No scaling needed; only the **order** of values matters.
- Interactions come for free: later questions depend on earlier answers.

## Choosing splits

| impurity | formula | used for |
|---|---|---|
| Gini | `1 - Σ pₖ²` | classification (default) |
| entropy | `-Σ pₖ log₂ pₖ` | classification (`criterion="entropy"`) |
| squared error | weighted variance | regression (default) |

Split score = size-weighted average impurity of the two groups. Candidate thresholds = midpoints between consecutive distinct values. The search is **greedy**: best split now, no look-ahead.

Fast regression splits: sort once, then `SSE = Σy² - (Σy)²/n` with cumulative sums.

## Controlling overfitting

| setting | effect |
|---|---|
| `max_depth` | limit the number of questions to a leaf |
| `min_samples_leaf` | every leaf needs this many examples (often the most useful) |
| `max_leaf_nodes` | limit the total number of leaves |
| `min_samples_split` | a node needs this many examples to split |
| `ccp_alpha` | prune the grown tree back (cost-complexity pruning) |

A single deep tree: low bias, **very high variance** (unstable).

## Bagging and random forests

```python
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier
forest = RandomForestRegressor(n_estimators=300, max_features=0.5, oob_score=True, n_jobs=-1, random_state=0)
forest.fit(X, y)
forest.oob_score_, forest.oob_prediction_         # free validation estimate
```

- **Bagging**: deep trees on bootstrap samples, averaged. Removes much of the variance.
- **Random forest**: also a random subset of features at each split (`max_features`), making trees less correlated. Defaults: `"sqrt"` for classification; for regression try 0.33 to 1.0.
- More trees never overfit; they just cost time. Diminishing returns after a few hundred.
- Each bootstrap sample misses about 37% of rows (`(1 - 1/n)ⁿ ≈ 1/e`): those are **out of bag**.
- OOB error matches random-fold cross-validation, **not** grouped or time-based validation.
- Fully grown forests can be very large in memory; `min_samples_leaf` shrinks them.

## Gradient boosting

```python
# by hand, squared error
prediction = np.full(len(y), y.mean())
for _ in range(n_trees):
    tree = DecisionTreeRegressor(max_depth=3).fit(X, y - prediction)    # fit the residuals
    prediction += learning_rate * tree.predict(X)
```

- Each shallow tree fits the **negative gradient** of the loss (for squared error, the residuals).
- Classification: work on the log-odds; residuals are `y - p`; leaf values use a Newton step `Σr / Σp(1-p)`.
- Small learning rate (0.05 to 0.1) + many trees + **early stopping** works best. Boosting **can** overfit with too many trees.

```python
from sklearn.ensemble import HistGradientBoostingRegressor
model = HistGradientBoostingRegressor(learning_rate=0.05, max_leaf_nodes=31, max_iter=3000,
                                      early_stopping=True, n_iter_no_change=50, random_state=0)
model.fit(X, y); model.n_iter_                       # trees actually used
model.validation_score_                              # validation score per iteration
```

- Histogram boosting bins each feature (at most 255 bins): much faster on large data.
- Handles **missing values** natively; `categorical_features=` for categories.
- The default `max_iter=100` can stop too early: use early stopping.
- The same family: **XGBoost**, **LightGBM**, **CatBoost** (`pip install`).

| setting | typical start |
|---|---|
| `learning_rate` | 0.05 to 0.1 |
| number of trees | early stopping |
| `max_leaf_nodes` / `max_depth` | 15 to 63 / 3 to 8 |
| `min_samples_leaf` | 20 |
| `l2_regularization` | 0, then 0.1 to 10 |

## Explaining tree ensembles

```python
from sklearn.inspection import permutation_importance, PartialDependenceDisplay
perm = permutation_importance(model, X_valid, y_valid, n_repeats=10, random_state=0)
PartialDependenceDisplay.from_estimator(model, X_valid, ["feature"], kind="both")   # average + ICE curves
```

- `feature_importances_` (impurity) is computed on training data and **favours features with many values**; it can rank noise highly.
- **Permutation importance on held-out data** is the reliable choice. Correlated features share credit.
- Partial dependence shows the **shape** of an effect; ICE curves show whether it differs between examples. Integer columns must be converted to float first.
- Importance describes the model, not causes.

## Limits

- **No extrapolation**: predictions stay within (forests) or near (boosting) the training range of the target, and go flat beyond the range of the features.
- Smooth or diagonal patterns become staircases; a linear model may win on small data with linear effects.
- Trees learn local detail well, which helps for familiar data and less for genuinely new regions or periods.

## Choosing a model for tables

1. Baseline. 2. Linear model with good features. 3. Random forest, defaults. 4. Gradient boosting with early stopping, then tune. 5. Compare honestly with validation that matches the real use.
