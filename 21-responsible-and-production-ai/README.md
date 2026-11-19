# Module 21: Responsible and Production AI

A model with a good test score is not yet a good system. This last module covers the two things that decide whether a model does good work in the real world. **Responsible AI**: whom the model fails, why it decides what it decides, and what it reveals about the people in its data. **Production**: tracking how a model was made, packaging and serving it safely, and noticing when the world it was trained on stops matching the world it works in.

Everything runs with scikit-learn, pandas and FastAPI on an ordinary computer or a free Colab session. The module ends with the course's final capstone.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | Who is in the data, fairness metrics, why fairness definitions conflict, proxies, mitigation with per-group thresholds, permutation importance and partial dependence, exact Shapley values, privacy (k-anonymity, differential privacy, membership inference), experiment tracking, model bundles and model cards, a tested FastAPI service, containers, and drift monitoring with PSI and the KS test | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/21-responsible-and-production-ai/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, from selection rates and a differentially private count to PSI, request validation, exact Shapley values and the KS statistic | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/21-responsible-and-production-ai/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/21-responsible-and-production-ai/03_solutions.ipynb) |
| 4 | Final capstone | From Notebook to Service: a bank's call-list model taken from raw data to a documented, tested, containerised service, then monitored through eighteen months of a changing economy | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/21-responsible-and-production-ai/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The data is described in [data/README.md](data/README.md).

The lesson and the capstone write their files (run logs, model bundles, model cards, the service code and a Dockerfile) to a `work` folder next to the notebooks. It's ignored by git, and you can delete it at any time.

## What you'll learn

**Responsible AI**

- **Auditing the data.** Group sizes, base rates, and what group labels can and can't capture.
- **Fairness metrics.** Demographic parity, equal opportunity, equalised odds and predictive parity, and why, with different base rates, they can't all hold at once.
- **Proxies.** Why removing a sensitive column rarely removes its influence, and makes auditing harder.
- **Mitigation.** Better data, reweighting, constraints and per-group thresholds, and the trade-offs each one brings.
- **Interpretability.** Permutation importance and partial dependence for the whole model; Shapley values for a single prediction, computed exactly.
- **Privacy.** Why removing names isn't anonymisation, k-anonymity, differential privacy with the Laplace mechanism, and membership inference attacks.

**Production**

- **Experiment tracking.** Logging parameters, metrics, data fingerprints and library versions for every run.
- **Packaging.** Saving the whole pipeline with its metadata, and writing a model card from the same numbers as the evaluation.
- **Serving.** A FastAPI service with validated inputs, tested like any other code.
- **Containers.** A Dockerfile with pinned versions, so the model runs the same everywhere.
- **Monitoring.** Input drift and prediction drift with the population stability index and the Kolmogorov-Smirnov test, and performance once labels arrive.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `selection_rates`, `disparate_impact`, `laplace_count`, `k_anonymity` |
| Medium | `error_rates_by_group`, `threshold_for_tpr`, `psi`, `reweighing_weights`, `validate_request` |
| Hard | `permutation_importance_scores`, `shapley_values`, `ks_statistic` |

The checks catch the classic mistakes:

- a disparate impact ratio that divides by a reference group with no positive predictions
- Laplace noise with scale `epsilon` instead of `1 / epsilon`
- groups with missing values silently dropped from a k-anonymity count
- an undefined error rate reported as 0 instead of `nan`
- a threshold that rounds down and misses the target true positive rate
- PSI bins that don't cover values outside the training range
- treating `True` as a number, or stopping at the first problem, when validating a request
- shuffling the caller's data in place when measuring permutation importance
- a KS statistic that only looks at one sample's values

## Final capstone

**From Notebook to Service.** The bank marketing data from Module 13, set in July 2009: the bank wants a model in production and will keep calling customers for another eighteen months.

- **Model choice with every run tracked**, and a metric that matches the decision: ranking customers called in the same month.
- **A fairness audit by age group**, written into a generated **model card** with the intended use and a monitoring plan.
- **Explanations**: permutation importance and an exact Shapley explanation for one customer.
- **A real service**: shared feature code, a FastAPI app, pytest tests (including one that rejects a leaking input), and a Dockerfile.
- **Monitoring** quarter by quarter: input drift, prediction drift, calibration and ranking once outcomes arrive, and a comparison of three responses to the alerts.
- **A final checklist** in code, and a wrap-up of the whole course.

## Prerequisites

Modules [1](../01-python-foundations) to [20](../20-llms-in-practice). Most important here: Module 11 (FastAPI), Module 13 (evaluating classifiers, and the bank data) and Module 14 (gradient boosting).

## Next

This is the last module. You've reached the end of the course: well done. The capstone ends with ideas for where to go from here.
