# Module 13: Evaluating Classifiers

A classifier that is "95% accurate" might be excellent, useless or dangerous. This module is about telling which: how common each class is, which mistakes a model makes and what they cost, how well it ranks cases, whether its probabilities can be trusted, where to set its threshold, and how sure you can be of any of it. Choosing the wrong metric is one of the most common ways real projects go wrong, so this is one of the most practical modules in the course.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | The confusion matrix, precision and recall, thresholds, ROC and precision-recall curves, costs, calibration, class imbalance, multiclass averages, bootstrap comparisons | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/13-evaluating-classifiers/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises: build the evaluation toolkit from scratch, from the confusion matrix to Platt scaling | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/13-evaluating-classifiers/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/13-evaluating-classifiers/03_solutions.ipynb) |
| 4 | Mini project | Who should the bank call? A real marketing campaign, judged by ranking, calibration and profit, and tested on the future | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/13-evaluating-classifiers/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The bank marketing data and its source are described in [data/README.md](data/README.md).

## What you'll learn

**Why accuracy misleads.** A test that calls everyone healthy is 99% accurate for a disease 1 in 100 people have. The base rate frames every number.

**The confusion matrix and its metrics.** True and false positives and negatives; precision, recall, specificity, F1 and F-beta; balanced accuracy and the Matthews correlation coefficient, which stay honest when classes are imbalanced.

**Scores and thresholds.** Classifiers produce scores, and the threshold that turns them into decisions is a choice, not a fixed 0.5.

**ROC and precision-recall curves.** AUC computed by hand and shown to equal the chance that a random positive outranks a random negative. An experiment where only the base rate changes: the ROC curve stays the same while precision collapses.

**Thresholds from costs.** Predict positive when `p > C_FP / (C_FP + C_FN)`, checked against the data. Budgets and precision at k when costs are unknown.

**Calibration.** Reliability diagrams, the Brier score and log loss; an overconfident naive Bayes model, fixed with `CalibratedClassifierCV`.

**Class imbalance.** What class weights really do (mostly move the threshold, and break the probabilities), and why moving the threshold yourself is usually better.

**More than two classes.** Per-class metrics, and macro, weighted and micro averages.

**How sure are you?** Paired bootstrap intervals for comparing two models on the same test cases, and the difference between a real difference and one that matters.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `confusion_counts`, `binary_metrics`, `f_beta`, `multiclass_confusion` |
| Medium | `averaged_f1`, `roc_curve_points`, `auc_by_ranks`, `average_precision`, `reliability_table` |
| Hard | `best_threshold`, `platt_scaling`, `paired_bootstrap` |

Most checks compare your code with scikit-learn's own metric, including the awkward cases. Others catch the classic mistakes:

- probabilities passed where labels were expected
- tied scores drawn as a staircase no threshold can reach
- an average that ignores a class the model never predicts
- an "unpaired" bootstrap that throws away most of its power

## Mini project

Which customers should a Portuguese bank call to sell a term deposit? 41,188 real calls from 2008 to 2010:

- **Leakage.** The call's length raises the AUC from 0.79 to 0.93, and is only known after the call. It's removed.
- **Accuracy is useless.** Both models beat "always no" by about one percentage point. Ranking tells the real story: calling the top 20% of customers reaches over 60% of subscribers.
- **Calibration.** Logistic regression and gradient boosting are well calibrated. Class weighting keeps the ranking and makes the probabilities more than three times too high.
- **Profit.** With illustrative costs (5 euros a call, 40 euros a subscription), the break-even probability of 0.125 is the best threshold. Calling everyone loses money; the model's rule makes a clear profit on the test set.
- **Honest intervals.** The profit is clearly positive, but the gap between the two models is small and not conclusive.
- **The future.** Trained on calls up to May 2009 and tested on the later ones, the AUC falls to about 0.6, the probabilities are badly off, and calling everyone beats both models. The module ends with what a careful team would do about that.

## Prerequisites

Modules [1](../01-python-foundations) to [12](../12-supervised-learning). Module 11 (scikit-learn, pipelines and cross-validation) and Module 12 (logistic regression) are used most, and Module 9's log loss and bootstrap come back.

## Next

Module 14: Trees and Ensembles (planned)
