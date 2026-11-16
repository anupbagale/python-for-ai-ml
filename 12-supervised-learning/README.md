# Module 12: Supervised Learning in Depth

The first module of Part 2. It slows down and looks at what's underneath every supervised model in the course: how linear models work, how good features stretch them, how to measure regression honestly, why models fail through **bias** or **variance**, and how **regularisation** keeps flexible models under control.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Linear regression three ways, metrics, features, bias and variance, ridge and lasso, learning curves, logistic regression in depth | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/12-supervised-learning/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, including ridge, the lasso and softmax regression written from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/12-supervised-learning/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the maths behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/12-supervised-learning/03_solutions.ipynb) |
| 4 | Mini project | What drives house values? Linear models on the 1990 California census | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/12-supervised-learning/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The housing data and its source are described in [data/README.md](data/README.md).

## What you'll learn

**Linear regression, three ways.** The normal equations, `lstsq` and scikit-learn give the same answer. Reading the weights, and why strongly correlated features (multicollinearity) make them unreliable.

**Measuring regression.** MAE, RMSE and R², what each one hides, why R² can be negative, and how residual plots show problems a single number can't.

**Features make linear models flexible.** Polynomials, interactions, logarithms, ratios and splines: still linear regression, but able to fit curves.

**Bias and variance.** Measured by simulation: expected error = bias² + variance + noise, and how flexibility trades one for the other.

**Regularisation.** Ridge written from its closed form, the lasso's exact zeros, coefficient paths, and choosing the penalty by cross-validation. Also an honest result: a penalty can stop useless features from hurting, but it can't make them help.

**Learning curves.** Telling high bias from high variance, and what to do about each.

**Logistic regression in depth.** Straight decision boundaries, softmax for several classes, the effect of regularisation, and polynomial features for curved boundaries.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `regression_metrics`, `polynomial_features`, `fit_least_squares`, `add_interactions` |
| Medium | `ridge_closed_form`, `choose_alpha`, `learning_curve_points`, `bias_variance`, `vif` |
| Hard | `lasso_coordinate_descent`, `softmax_regression`, `target_encode_oof` |

Several checks compare your code with scikit-learn's own implementation, so a pass means your version computes exactly the same thing. Others catch the classic mistakes:

- normal equations that lose accuracy on nearly identical columns
- a penalised intercept
- a scaler fitted outside cross-validation
- target encoding that leaks the answer: on pure noise, it fakes a strong signal

## Mini project

Predicting median house values for 19,675 California block groups from the 1990 census, using linear models only:

- **Audit.** House values are capped at $500,001 and ages at 52. The capped targets are removed, and the model is clearly limited to values under $500,000.
- **A ladder of models**, each step decided by cross-validation:
  - Per-household ratios alone make a linear model **worse**, because of extreme outliers.
  - Logarithms fix that.
  - Smooth curves for location help more.
- **A collapse.** A 210-feature surface over the map gives errors in the millions under plain least squares. With ridge, it becomes the best model.
- **Geography and validation.** Holding out whole map regions costs $5,000 to $6,000 more error, and favours a stronger penalty. What "new data" means changes the answer.
- **Results.** Test RMSE about $56,000 and MAE about $38,000. Similar accuracy in every region, with small regional biases. Income is by far the strongest effect.
- **A preview of Module 14.** Untuned gradient boosting reaches about $44,000, because trees find interactions a linear model can't.

## Prerequisites

Modules [1](../01-python-foundations) to [11](../11-capstone). Modules 6 (NumPy), 9 (Math in Code) and 11 (scikit-learn and cross-validation) are used most.

## Next

Module 13: Evaluating Classifiers (planned)
