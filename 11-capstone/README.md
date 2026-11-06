# Module 11: Capstone

The last module of Part 1 puts everything so far together. It starts from the original, raw field records of the Palmer Station penguin study and ends with a tested web service that predicts a penguin's sex from four measurements, and says "unsure" when it should.

The lesson introduces the last tools needed: scikit-learn, pipelines, cross-validation, honest tuning, saving models, and serving them with FastAPI.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | scikit-learn, pipelines, leakage, cross-validation, tuning, saving models, and a FastAPI service | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/01_lesson.ipynb) |
| 2 | Exercises | 12 building blocks of the capstone, from cleaning raw columns to an API | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with notes on what goes wrong in real projects without them | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/03_solutions.ipynb) |
| 4 | Capstone | A penguin sexing service, from raw records to a running API | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/11-capstone/04_capstone.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The raw dataset and its license are described in [data/README.md](data/README.md).

The notebooks install FastAPI, httpx and uvicorn automatically if they're missing; Colab already has everything else.

## What you'll learn

**The shape of an ML system.** The stages, from data collection to monitoring, and how each one fails.

**scikit-learn.** The `fit` / `predict` / `transform` interface, checked against the logistic regression you built by hand in Module 9. The weights agree to about five decimal places.

**Pipelines.** `ColumnTransformer` and `Pipeline` bundle imputing, scaling and encoding with the model, so preprocessing is refitted inside cross-validation and saved with the model.

**Leakage.** Selecting features before cross-validation finds 87% accuracy in pure random noise. Inside a pipeline, the same procedure gives 50%, the right answer.

**Cross-validation and model comparison.** Stratified k-fold, baselines, and why the simplest model within the noise is usually the right choice.

**Honest tuning.** `GridSearchCV` to choose hyperparameters, and nested cross-validation to report them without optimism.

**Saving and serving.** Saving models with joblib, and why you should only load trusted files. A FastAPI service with Pydantic validation, tested with `TestClient`.

**After deployment.** Logging predictions, watching for drift, saying "unsure", and model cards.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `snake_case`, `clean_species`, `clean_sex`, `out_of_range` |
| Medium | `make_preprocessor`, `compare_models`, `selective_accuracy`, `PenguinIn`, `drift_report` |
| Hard | `nested_cv_score`, `permutation_importance`, `create_app` |

## The capstone

The brief: the field team wants to sex penguins from a minute's measurements instead of a blood sample and a DNA test. The tool must say "unsure" rather than guess, and it must work on future seasons. The notebook covers:

1. **Audit the raw data.** It has inconsistent species names, constant columns, failed sexing tests explained in comments, and an "ID" column that turns out to label nests, not birds.
2. **Clean and validate** it with the functions from the exercises.
3. **Choose features by what's available in the field.** The blood isotope measurements are excluded: a tool that needs a blood sample to avoid needing a blood sample is useless.
4. **Train on 2007 and 2008, and hold out 2009** to test the way the tool will really be used.
5. **Compare five model types with cross-validation.** All of them land within a few points of each other, so the simplest well-behaved one, logistic regression, is chosen.
6. **Tune with nested cross-validation.** It shows the grid search choosing between ties.
7. **Pick an "unsure" margin** from out-of-fold predictions, aiming for 97% accuracy on the answers it gives.
8. **Test once on 2009.**
9. **Check for drift.**
10. **Save a model bundle and generate a model card** from it.
11. **Serve and test it.** A FastAPI service with validation, warnings and logging, an 11-test suite, and a real uvicorn server answering HTTP requests.

The results, as they came out:

- Answering every bird, accuracy on the new season is 88%, against about 92% expected from cross-validation.
- With "unsure", the model answers 85% of birds and gets 96% of those right.
- The drift check finds flipper lengths longer in 2009 for every species, which is worth asking the field team about.

## Prerequisites

All of Modules [1](../01-python-foundations) to [10](../10-engineering-practices).

## That's Part 1

The same steps (look at the data carefully, decide what the tool can know, evaluate honestly, package it with everything it needs, and keep checking it) apply to any problem you take on next. Part 2 of the [roadmap](../README.md#roadmap) goes deeper into machine learning itself, and Part 3 into deep learning and modern AI.
