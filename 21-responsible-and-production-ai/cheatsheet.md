# Module 21 Cheatsheet

## Auditing the data

```python
df.groupby("group")["target"].agg(people="size", rate="mean")
```

- Check group sizes (small groups give noisy numbers) and base rates (different rates shape everything).
- Group labels are simplifications; say so.

## Fairness metrics

| definition | equal across groups | question |
|---|---|---|
| demographic parity | selection rate `P(pred = 1)` | positive outcome equally often? |
| equal opportunity | true positive rate | qualified people recognised equally often? |
| equalised odds | TPR **and** FPR | both mistakes equally common? |
| predictive parity | precision | a "yes" equally reliable? |

- Disparate impact ratio: selection rate / reference group's rate; below 0.8 is a common warning sign (the "four-fifths rule").
- With different base rates, calibration, equal TPR and equal FPR can't all hold at once (unless the model is perfect).
- Choosing a definition is a decision about the use and its values, not a technical one.

```python
def group_report(y_true, y_pred, groups):
    rows = {}
    for g in pd.unique(groups):
        m = np.asarray(groups == g)
        t, p = np.asarray(y_true)[m], np.asarray(y_pred)[m]
        rows[g] = {"selection": p.mean(), "TPR": p[t == 1].mean(), "FPR": p[t == 0].mean(), "precision": t[p == 1].mean()}
    return pd.DataFrame(rows).T
```

## Proxies and mitigation

- Dropping a sensitive column doesn't remove its influence: other columns predict it (test: train a model to predict the column).
- Keep sensitive columns for **auditing**, even if the model doesn't use them.
- Before training: better data, reweighting (`weight = P(group) P(label) / P(group, label)`).
- During training: fairness constraints (for example, the Fairlearn library).
- After training: per-group thresholds (legally restricted in some settings).
- Every mitigation moves some other number the wrong way; report both.

## Explaining models

```python
from sklearn.inspection import permutation_importance, partial_dependence

result = permutation_importance(model, X_val, y_val, scoring="roc_auc", n_repeats=5, random_state=0)
pd_result = partial_dependence(model, X_val, ["age"], grid_resolution=30)   # numeric columns as float
```

- Permutation importance: drop in score when one column is shuffled. Use held-out data. Correlated columns share importance.
- Partial dependence: average prediction as one column varies.
- Shapley values: a feature's average contribution over all orders of adding features; they add up to prediction minus average prediction. Exact cost: `2 ** d` subsets.
- Shapley weight for a subset `S` without feature `j`: `|S|! (d - |S| - 1)! / d!`.
- Explanations describe the model, not the world, and don't justify it.

## Privacy

- Quasi-identifiers (age, postcode, sex...) can single people out without names.
- k-anonymity: every combination of quasi-identifiers shared by at least `k` records. `df.groupby(cols, dropna=False).size().min()`
- Differential privacy for a count: `count + rng.laplace(0, 1 / epsilon)`. Smaller `epsilon`: more privacy, more noise.
- Overfitted models leak membership; regularisation helps privacy too.
- Collect only what you need; never put personal data in a public repository.

## Experiment tracking

```python
def fingerprint(frame):
    return hashlib.sha256(pd.util.hash_pandas_object(frame, index=True).values.tobytes()).hexdigest()[:16]

record = {"run": name, "params": params, "metrics": metrics, "train_data": fingerprint(train),
          "versions": {"scikit-learn": sklearn.__version__}}
with open("runs.jsonl", "a") as f:
    f.write(json.dumps(record) + "\n")
```

- Log every run, including failures, with fairness metrics next to accuracy.
- Tools: MLflow, Weights & Biases.

## Packaging and model cards

```python
bundle = {"pipeline": model, "threshold": 0.5, "version": "1.0", "categories": {...},
          "train_data": fingerprint(train), "versions": {"scikit-learn": sklearn.__version__}}
joblib.dump(bundle, "model.joblib")
```

A model card says: what the model does, **intended and non-intended uses**, training data, evaluation, performance **by group**, known limitations, and the monitoring plan. Generate it from the evaluation's numbers.

## Serving

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, ConfigDict, Field

class Customer(BaseModel):
    model_config = ConfigDict(extra="forbid")     # unexpected fields are rejected (422)
    age: int = Field(ge=17, le=100)
    job: str

app = FastAPI()

@app.post("/score")
def score(customer: Customer):
    row = customer.model_dump()
    if row["job"] not in bundle["categories"]["job"]:
        raise HTTPException(status_code=422, detail="unknown job")
    ...
```

- Put feature preparation in one file that training and serving both import (avoids training/serving skew).
- Test the service: valid input, out-of-range, unknown category, missing field, extra field, and "same answer as the notebook".
- `from fastapi.testclient import TestClient`; run tests with `pytest`.

## Containers

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY features.py app.py model.joblib ./
EXPOSE 8000
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

- Pin exact versions of the libraries the model was trained with.
- `docker build -t name .` then `docker run -p 8000:8000 name`.

## Monitoring

```python
def psi(expected, actual, bins=10):
    edges = np.unique(np.quantile(expected, np.linspace(0, 1, bins + 1)))
    edges[0], edges[-1] = -np.inf, np.inf
    e = np.clip(np.histogram(expected, edges)[0] / len(expected), 1e-4, None)
    a = np.clip(np.histogram(actual, edges)[0] / len(actual), 1e-4, None)
    return float(np.sum((a - e) * np.log(a / e)))

from scipy import stats
stats.ks_2samp(train_col, live_col).statistic
```

| PSI | meaning |
|---|---|
| below 0.1 | no meaningful change |
| 0.1 to 0.25 | worth a look |
| above 0.25 | significant shift |

- Columns with many equal values (mostly 0): choose bin edges yourself. Categories: compare shares.
- Watch inputs and predictions all the time; performance and fairness when labels arrive.
- Evaluate with the metric that matches the decision (for example, ranking within each month's list).
- Diagnose before acting: if only calibration broke, recalibrate (shift the log-odds); if the ranking broke, retrain.
- Write the plan down: what is checked, how often, alert thresholds, who is told, what happens next.
