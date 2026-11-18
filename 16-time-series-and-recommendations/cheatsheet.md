# Module 16 Cheatsheet

## Time series basics

```python
s = df.set_index("date")["value"].asfreq("D")     # regular daily index; gaps become NaN
s.isna().sum()                                     # always check for gaps
s.rolling(30, center=True).mean()                  # smooth to see the trend
s.autocorr(lag=7)                                  # correlation with the value 7 steps earlier
```

- Order matters, rows depend on each other, and only the past is available at forecast time.
- **Trend** (slow direction), **seasonality** (fixed-period repeats), **noise**. Additive: a fixed amount; multiplicative: scales with the level (divide by the trend).
- Library decomposition: `statsmodels.tsa.seasonal.STL`.

## Baselines and metrics

| forecast | code |
|---|---|
| naive | `s.shift(1)` |
| seasonal naive | `s.shift(7)` (daily data, weekly season) |
| moving average | `s.shift(1).rolling(7).mean()` |
| recent profile | average of the same hour on recent days of the same type |

- **MAE** (in units), **RMSE** (punishes big misses).
- **MAPE** explodes near zero and is asymmetric.
- **MASE** = MAE / MAE of seasonal naive on the training data; below 1 beats naive; comparable across series.

## Exponential smoothing

```python
level = alpha * y[t] + (1 - alpha) * level        # forecast for t + 1 = level
s.ewm(alpha=0.3, adjust=False).mean().shift(1)     # the same, in pandas
```

- Holt: adds a trend (`level + h * trend`). Holt-Winters: adds seasonality. Damped trend for long horizons.
- `statsmodels`: `ExponentialSmoothing`, `ARIMA`. Book: *Forecasting: Principles and Practice* (Hyndman and Athanasopoulos).

## Validation

```python
from sklearn.model_selection import TimeSeriesSplit
TimeSeriesSplit(n_splits=5, test_size=60, gap=7)      # train on the past, test on the next block
```

- Never shuffle. Random K-fold is far too optimistic for time series.
- `gap` when features or targets span several steps; `max_train_size` for a sliding window.

## Forecasting with features

```python
X = pd.DataFrame({f"lag_{k}": s.shift(k) for k in [1, 7, 14]})
X["mean_7"] = s.shift(1).rolling(7).mean()             # SHIFT FIRST, then roll
X["weekday"] = s.index.dayofweek
model = HistGradientBoostingRegressor(loss="poisson")  # Poisson for counts
```

- Every feature: **would I know this at forecast time?** Weather: use forecasts, not recorded values.
- **Recursive**: feed forecasts back as inputs (errors compound). **Direct**: one model per horizon, only lags ≥ horizon.
- Errors grow with the horizon.
- Trees **can't extrapolate** a trend: model the ratio to a recent level or the differences; leave out a "time since start" feature.
- Special days (holidays, the days around them, storms) need calendar features or manual overrides.

## Prediction intervals

```python
low = HistGradientBoostingRegressor(loss="quantile", quantile=0.1).fit(X, y)
high = HistGradientBoostingRegressor(loss="quantile", quantile=0.9).fit(X, y)
```

- **Check coverage** on test data. Quantile models are often overconfident on new periods.
- **Conformal calibration**: on a recent held-out period, find how far actuals fall outside; widen by the 80% quantile of that.

## Recommender basics

- Users × items, mostly empty. **Explicit** (ratings) or **implicit** (purchases, clicks: 0 means "not seen", not "disliked").
- Baseline: `μ + b_user + b_item`, with biases shrunk: `Σ(r - μ) / (n + λ)`.

## Collaborative filtering

```python
sim = (R.T @ R) / np.outer(norms, norms)           # item-item cosine; set the diagonal to 0
scores = R[user] @ sim                              # implicit: total similarity to what the user has
```

For ratings: subtract the biases first; predict with a weighted average of the user's ratings for the most similar items.

## Matrix factorisation

- `r̂ = μ + b_u + b_i + p_u · q_i`, with `f` factors per user and item.
- **ALS**: fix `Q`, solve a ridge regression for each user: `p_u = solve(QᵤᵀQᵤ + λI, Qᵤᵀ r_u)`; then fix `P` and do items; repeat.
- Too many factors or too little regularisation overfits.
- Implicit data: `TruncatedSVD` on the 0/1 matrix, or implicit ALS (Hu, Koren and Volinsky, 2008).

## Evaluating recommenders

- Split **by time**: train on the past, test on what users did next.
- **precision@k**, **recall@k**, **hit rate@k**, and **coverage** (how many different items are recommended).
- Always compare with **popularity** and, for shops, **buy again**. Decide whether repeats count.
- Offline metrics on logged behaviour **favour popular items** (data missing not at random). The final test is an online experiment.
- Cold start: new users and items need popularity, content features or questions.
- Ranking: exclude seen items; break ties by popularity then index.
