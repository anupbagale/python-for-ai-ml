# Module 14: Trees and Ensembles

On tables of data, the models that win most often in practice are ensembles of decision trees: random forests and gradient boosting. This module builds them up from a single tree, which is easy to understand and easy to break, through the two ideas that make trees strong: **averaging** many of them (bagging and random forests) and **correcting** them one after another (boosting). It ends with how to explain what a tree ensemble has learnt, and where trees still fall short.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Trees and splits, impurity, overfitting and pruning, regression trees, bagging, random forests, importance done right, gradient boosting by hand, histogram boosting, extrapolation | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/14-trees-and-ensembles/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, ending with a decision tree and gradient boosting written from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/14-trees-and-ensembles/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/14-trees-and-ensembles/03_solutions.ipynb) |
| 4 | Mini project | The rematch: trees and ensembles against Module 12's best linear model on California housing | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/14-trees-and-ensembles/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The mini project uses the California housing data from [Module 12](../12-supervised-learning/data/README.md); no new data is needed.

## What you'll learn

**A tree is a set of questions.** Reading a tree's rules, and why every boundary is horizontal or vertical.

**How a tree chooses a question.** Gini impurity and entropy, and the best split found by hand, matching scikit-learn's.

**Growing, stopping and pruning.** Overfitting as a tree deepens, and the settings that control it: depth, leaf size, number of leaves, cost-complexity pruning.

**Regression trees.** Step functions, and the same trade-off seen as the shape of a curve.

**Instability and bagging.** Five bootstrap samples give five very different trees; averaging them removes most of the extra error. Out-of-bag predictions as a free validation set.

**Random forests.** Random feature subsets make trees less alike, so their average works better.

**Importance done right.** An experiment where impurity importance ranks pure noise as the most important feature, and permutation importance on held-out data gets it right. Partial dependence for the shape of an effect.

**Gradient boosting.** Written by hand in a few lines: each small tree fits the mistakes so far. Learning rate, number of trees and early stopping; histogram boosting, missing values, and the XGBoost, LightGBM and CatBoost family.

**What trees can't do.** They can't extrapolate beyond the training range, and they approximate smooth trends with steps.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `gini_impurity`, `entropy`, `weighted_impurity`, `predict_with_tree` |
| Medium | `best_split`, `best_split_regression`, `bagging_with_oob`, `permutation_importance_scores`, `partial_dependence_1d` |
| Hard | `build_tree`, `gradient_boosting_regressor`, `binary_gradient_boosting` |

Your `build_tree` must make the same predictions as scikit-learn's `DecisionTreeClassifier`, and your boosting models are compared with scikit-learn's. Other checks catch the classic mistakes:

- a threshold placed on a data value instead of between two values
- a split between equal values, which no threshold can make
- shuffling the caller's data instead of a copy
- boosting that fits the targets instead of the residuals, or forgets the learning rate

## Mini project

Module 12's carefully engineered linear model against trees, on the same data, split and folds:

- **A single tree**, with only its leaf size tuned and no feature engineering, nearly matches the whole linear ladder on familiar neighbourhoods, but is clearly worse on unseen regions.
- **A random forest** cuts the error by about 15%, and its free out-of-bag estimate agrees with cross-validation, but only for the random-fold question.
- **Gradient boosting** with early stopping cuts it by almost a quarter. The default of 100 trees turns out to stop too early; once early stopping chooses the number of trees, the other settings barely matter.
- **On unseen regions**, every model is worse and the gains of the ensembles shrink to about a third, because much of what trees learn is local detail.
- **Inside the model**, permutation importance and partial dependence show it relies mostly on location and income.
- **On the test set**, gradient boosting has an RMSE about $14,000 lower than the linear model, with a paired bootstrap interval far from zero.
- **The costs**: no tree prediction goes beyond the training range, and the forest takes over 100 MB against 2.4 MB for the more accurate boosting model.

## Prerequisites

Modules [1](../01-python-foundations) to [13](../13-evaluating-classifiers). Module 12 (bias and variance, the housing data) and Module 13 (honest comparisons) are used most.

## Next

Module 15: Unsupervised Learning (planned)
