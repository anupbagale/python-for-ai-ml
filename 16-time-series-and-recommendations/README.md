# Module 16: Time Series and Recommendations

Two kinds of problem with one hard rule: **the test set is the future**. Time series forecasting predicts the next values of a quantity from its past: demand, sales, traffic. Recommender systems predict what a person will want next, from a huge, mostly empty table of who liked or bought what. Both break the assumptions of ordinary cross-validation, and both are everywhere in practice.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Trend and seasonality, autocorrelation, baselines, exponential smoothing, time-ordered validation, lag features and leaks, multi-step forecasts, extrapolation, calibrated intervals; ratings, biases, collaborative filtering, matrix factorisation, evaluating recommenders | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/16-time-series-and-recommendations/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises: forecasting tools, then a recommender built from its parts | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/16-time-series-and-recommendations/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/16-time-series-and-recommendations/03_solutions.ipynb) |
| 4 | Mini project | A day-ahead forecast of hourly bike demand, and product recommendations for an online shop, both tested on the future | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/16-time-series-and-recommendations/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The datasets and their sources are described in [data/README.md](data/README.md).

## What you'll learn

**Part 1: time series**

- **What's different.** Order, dependence between observations, and only the past being available. Regular indexes and gaps.
- **Trend, seasonality and noise.** A decomposition by hand; additive and multiplicative seasons; autocorrelation.
- **Baselines.** Naive, seasonal naive and moving averages; MAE, RMSE, why MAPE misbehaves, and MASE.
- **Exponential smoothing.** Built by hand; Holt, Holt-Winters and ARIMA in context.
- **Honest validation.** Expanding-window splits, and how much random K-fold flatters a forecasting model.
- **Forecasting as supervised learning.** Lag, rolling and calendar features, and a leak caught in the act.
- **Many steps ahead.** Recursive and direct strategies; error growing with the horizon.
- **Extrapolation.** Trees trained on 2011 can't follow 2012's growth until the target becomes a ratio to the recent level.
- **Uncertainty.** Quantile regression intervals that turn out overconfident, fixed with conformal calibration.

**Part 2: recommendations**

- **The problem.** Sparse user × item tables, explicit and implicit feedback, and a ratings dataset made from a known model of taste.
- **Baselines.** Global mean and shrunk user and item biases.
- **Collaborative filtering.** Item-based similarities, checked against the true genres.
- **Matrix factorisation.** Alternating least squares written by hand; the learnt factors recover hidden genres.
- **Evaluation.** Precision and hit rate at k, and a striking lesson: offline metrics favour popular items even when they're not what people would like most.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `lag_features`, `rolling_features`, `seasonal_naive`, `precision_recall_at_k` |
| Medium | `forecast_errors`, `expanding_window_splits`, `exponential_smoothing`, `recursive_forecast`, `item_cosine_similarity` |
| Hard | `holt_forecast`, `als_factorise`, `recommend_top_k` |

The checks compare with pandas and scikit-learn where possible, and catch the classic mistakes:

- a rolling window that includes the value being forecast
- lags fed to a model in the wrong order
- Holt's trend updated with the wrong level
- recommending items the user has already seen
- ties broken arbitrarily for new users

## Mini project

**Part A: tomorrow's bike demand, hour by hour.**

- **The data.** Two years of hourly rentals, with missing hours filled (most are nights with no rentals; the rest are storm shutdowns).
- **Baselines.** A simple **recent profile** (the average of the same hour over recent days of the same type) beats "same hour yesterday" easily. Without weather, it beats gradient boosting on the summer validation period and loses on the autumn test, a lesson in why one validation period isn't enough.
- **With weather**, gradient boosting cuts the test error by about 40%, as an upper limit, since it uses recorded rather than forecast weather.
- **The worst days** (the Thanksgiving weekend, a hurricane, a day of heavy rain) point to better calendar and weather information, not a better model.

**Part B: what will this customer buy next?**

- **Cleaning.** Real transactions from a UK online shop, with cancellations, missing customers and postage charges removed.
- **Define the task first.** With repeat purchases allowed, a simple "buy again" list wins.
- **Discovering new products.** Item-based filtering and matrix factorisation roughly double the precision of popularity and show far more of the catalogue.
- **The limits.** New customers, seasons the model never saw, and the effect of recommendations themselves.

## Prerequisites

Modules [1](../01-python-foundations) to [15](../15-unsupervised-learning). Module 12 (ridge regression), Module 13 (honest evaluation), Module 14 (gradient boosting and its limits) and Module 15 (factorising a matrix) are used most.

## Next

This completes Part 2. Part 3 starts with Module 17: Deep Learning with PyTorch (planned).
