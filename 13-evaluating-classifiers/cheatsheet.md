# Module 13 Cheatsheet

## First questions

1. What is the **base rate** (how common is the positive class)?
2. What will the scores be used for: **ranking** a list, **yes/no decisions**, or **reporting risks**?
3. What does each kind of mistake **cost**?

## The confusion matrix

| | predicted negative | predicted positive |
|---|---|---|
| **actually negative** | TN | FP (false alarm) |
| **actually positive** | FN (miss) | TP |

```python
from sklearn.metrics import confusion_matrix
tn, fp, fn, tp = confusion_matrix(y_true, y_pred).ravel()     # rows: actual, columns: predicted
```

## Metrics from the matrix

| metric | formula | question |
|---|---|---|
| recall (sensitivity, TPR) | TP / (TP + FN) | what fraction of positives did we find? |
| precision (PPV) | TP / (TP + FP) | when we say positive, how often are we right? |
| specificity (TNR) | TN / (TN + FP) | what fraction of negatives did we clear? |
| false positive rate | FP / (FP + TN) | what fraction of negatives raised an alarm? |
| accuracy | (TP + TN) / all | misleading when classes are imbalanced |
| F1 | 2PR / (P + R) | harmonic mean, pulled towards the smaller |
| F-beta | (1 + β²)PR / (β²P + R) | β > 1 favours recall, β < 1 precision |
| balanced accuracy | (recall + specificity) / 2 | 0.5 for a model that ignores a class |
| MCC | correlation of labels | -1 to 1; 0 = chance; uses all four boxes |

```python
from sklearn.metrics import (classification_report, precision_score, recall_score, f1_score, fbeta_score,
                             balanced_accuracy_score, matthews_corrcoef)
print(classification_report(y_true, y_pred, digits=3))
precision_score(y_true, y_pred, zero_division=0)     # what to report when nothing is predicted positive
```

## Scores and thresholds

```python
p = model.predict_proba(X)[:, 1]          # scores, not labels
pred = p >= threshold                     # predict() uses 0.5, rarely the right choice
p_cv = cross_val_predict(model, X, y, cv=cv, method="predict_proba")[:, 1]   # honest scores for every row
```

Lower threshold: more recall, less precision. Choose it on validation data, never on the test set.

## ROC and AUC

```python
from sklearn.metrics import roc_curve, roc_auc_score
fpr, tpr, thresholds = roc_curve(y_true, p)
roc_auc_score(y_true, p)
```

- AUC = probability a random positive scores higher than a random negative (ties count half) = Mann-Whitney U / (n_pos · n_neg).
- Rank formula: `(sum of positives' ranks - n_pos(n_pos + 1)/2) / (n_pos · n_neg)`.
- 0.5 = random, 1 = perfect. Ignores the threshold, calibration and the base rate.

## Precision-recall and average precision

```python
from sklearn.metrics import precision_recall_curve, average_precision_score
precision, recall, thresholds = precision_recall_curve(y_true, p)
average_precision_score(y_true, p)        # step-function area; baseline = base rate
```

Use when positives are rare and acting on an alert has a cost. Falls as the base rate falls, even for the same model.

## Thresholds from costs

Predict positive when `p > C_FP / (C_FP + C_FN)`. With a gain `V` per success and a cost `C` per action: act when `p > C / V`. Needs **calibrated** probabilities.

With a fixed budget of `k` actions: take the top `k` scores. Report **precision at k**, **lift** (= precision at k / base rate), and a **gains chart** (share of positives reached against share of cases acted on).

## Calibration

```python
from sklearn.calibration import calibration_curve, CalibratedClassifierCV
frac_pos, mean_pred = calibration_curve(y_true, p, n_bins=10, strategy="uniform")   # or "quantile"
calibrated = CalibratedClassifierCV(model, method="sigmoid", cv=5)    # "isotonic" with more data
from sklearn.metrics import brier_score_loss, log_loss
```

- Reliability diagram: points on the diagonal = calibrated.
- **Brier score** = mean (p - y)². **Log loss** punishes confident mistakes hard. Both are proper scoring rules.
- **ECE** = Σ (bin count / n) · |fraction positive - mean predicted|; depends on the bins.
- Logistic regression is usually close; naive Bayes is overconfident; forests can be underconfident near 0 and 1.
- Calibrate on data the model didn't train on. Calibration breaks when the base rate changes.

## Class imbalance

- `class_weight="balanced"` and resampling mostly **move the threshold** and **break calibration**; the ranking barely changes.
- Usually better: train normally, then choose the threshold.
- Never resample validation or test data.
- The real problem is **few positives**, not the ratio.

## More than two classes

```python
f1_score(y_true, y_pred, average="macro")      # every class counts equally
f1_score(y_true, y_pred, average="weighted")   # weighted by support
f1_score(y_true, y_pred, average="micro")      # pooled counts; equals accuracy for single-label data
```

A big gap between macro and micro means some class is handled badly.

## Comparing models

```python
rng = np.random.default_rng(0)
for _ in range(2000):
    idx = rng.integers(0, n, n)                          # the same cases for both models: paired
    diffs.append(metric(y[idx], p_a[idx]) - metric(y[idx], p_b[idx]))
np.percentile(diffs, [2.5, 97.5])
```

Paired intervals are much narrower than two separate ones. An interval measures test-set noise only, not training noise or future drift. Ask whether a difference is big enough to change decisions.

## Common mistakes

- Reporting accuracy without the base rate.
- Using features known only after the outcome (leakage), like a call's duration.
- Choosing the threshold on the test set.
- Reading probabilities from a class-weighted model as real probabilities.
- Evaluating time-ordered data with a random split, then expecting the same results in the future.
- Declaring a winner from a difference smaller than the noise.
