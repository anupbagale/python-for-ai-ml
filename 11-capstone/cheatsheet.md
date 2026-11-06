# Module 11 Cheatsheet

## The ML system checklist

| stage | ask |
|---|---|
| data | How was it recorded? Are the IDs really IDs? What do the comments say? |
| validation | Types, ranges, allowed values, missing counts, all reported |
| features | Will this be available **at prediction time**? |
| split | What does "new data" mean here? Later time? New people? New sites? |
| selection | Cross-validation on training data only; a baseline; simplest within the noise |
| evaluation | One look at the test set, with an interval |
| packaging | Pipeline + metadata + versions in one bundle |
| serving | Validate input, allow "unsure", log every prediction |
| monitoring | Drift checks, and compare with true labels as they arrive |

## scikit-learn basics

```python
model.fit(X, y)                     # returns the model
model.predict(X); model.predict_proba(X)
model.classes_                      # column order of predict_proba
model.coef_, model.intercept_       # learned values end in _
LogisticRegression()                # regularised by default: C=1.0; C=np.inf for none
```

## Pipelines

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler

pre = ColumnTransformer([
    ("num", make_pipeline(SimpleImputer(strategy="median"), StandardScaler()), numeric_cols),
    ("cat", OneHotEncoder(handle_unknown="ignore", sparse_output=False), categorical_cols),
])                                   # other columns are dropped
model = make_pipeline(pre, LogisticRegression(max_iter=1000))
model.fit(df, y)                     # takes the raw DataFrame
model[:-1].get_feature_names_out()
```

**Anything that learns from data goes inside the pipeline.**

## Cross-validation

```python
from sklearn.model_selection import StratifiedKFold, cross_val_score, cross_validate, cross_val_predict

cv = StratifiedKFold(n_splits=10, shuffle=True, random_state=0)   # reuse for every model
scores = cross_val_score(model, X, y, cv=cv)                       # report mean and std
cross_validate(model, X, y, cv=cv, scoring=["accuracy", "recall"])
p_oof = cross_val_predict(model, X, y, cv=cv, method="predict_proba")[:, 1]   # for thresholds
```

`GroupKFold` when rows share an entity; a time-based split when the model will be used on the future.

## Baselines and candidates

```python
DummyClassifier()                          # most common class: must beat this clearly
LogisticRegression(), KNeighborsClassifier(n_neighbors=15)
RandomForestClassifier(n_estimators=300, random_state=0)
HistGradientBoostingClassifier(random_state=0)
```

When models are within one standard deviation, choose by simplicity, explainability and well-behaved probabilities.

## Tuning

```python
search = GridSearchCV(model, {"logisticregression__C": [0.01, 0.1, 1, 10]}, cv=cv).fit(X, y)
search.best_params_, search.best_score_      # best_score_ is optimistic
search.best_estimator_                       # refitted on all of X
nested = cross_val_score(search, X, y, cv=outer_cv)   # the honest estimate
```

Step names in `make_pipeline` are the lowercase class names: `logisticregression__C`.

## Leakage checklist

- preprocessing fitted on all data before splitting
- feature selection outside cross-validation
- features from the future, or only available after the outcome
- the same entity in train and test
- choosing anything (thresholds, features, models) by looking at the test set

## Saving

```python
import joblib
joblib.dump({"model": model, "version": "1.0.0", "metrics": ..., "versions": ...}, "bundle.joblib")
bundle = joblib.load("bundle.joblib")       # only files you trust: loading can run code
```

## Saying "unsure"

```python
covered = np.abs(p - 0.5) >= margin         # answer only these
# choose margin on out-of-fold predictions, never on the test set
```

## FastAPI

```python
from typing import Literal
from fastapi import FastAPI
from pydantic import BaseModel, ConfigDict, Field

class PenguinIn(BaseModel):
    model_config = ConfigDict(extra="forbid")
    species: Literal["Adelie", "Chinstrap", "Gentoo"]
    body_mass_g: float = Field(ge=2000, le=7000)

class Batch(BaseModel):
    penguins: list[PenguinIn] = Field(min_length=1, max_length=100)

def create_app(bundle) -> FastAPI:
    app = FastAPI(title="...", version=bundle["version"])

    @app.get("/health")
    def health():
        return {"status": "ok"}

    @app.post("/predict", response_model=PredictionOut)
    def predict(penguin: PenguinIn):
        frame = pd.DataFrame([penguin.model_dump()])
        ...
    return app
```

Invalid input gets **422** automatically. Test without a server:

```python
from fastapi.testclient import TestClient
client = TestClient(create_app(bundle))
client.post("/predict", json={...}).json()
```

Run for real: `uvicorn app:app --port 8000`, then open `/docs`.

## Monitoring

- Log input, output, model version and time for every prediction.
- Drift: `(new mean - train mean) / train std` and the fraction outside the training range, per column (per group if the mix changes).
- Don't retune on the test set when results disappoint; report, monitor, and retrain on evidence.
- Write a model card: what, data, evaluation, limitations, intended use.
