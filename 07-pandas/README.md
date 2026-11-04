# Module 7: pandas

DataFrames for loading, cleaning, exploring, joining and reshaping tabular data, the step before almost every classical ML model. The lesson redoes the Module 5 penguin pipeline in about twenty lines, so you can see exactly what pandas does for you, and the exercises and mini project focus on the traps that make pandas code silently wrong.

Everything is tested on both pandas 2.2 and pandas 3.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | From Series and DataFrames to the Module 5 pipeline in pandas | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/01_lesson.ipynb) |
| 2 | Exercises | 12 everyday pandas jobs, with checks that compare whole tables | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions with notes on the trap behind each one | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/03_solutions.ipynb) |
| 4 | Mini project | Predicting customer churn from raw order tables | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/07-pandas/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision.

## What you'll learn

**Series and DataFrames.** Columns with their own dtypes, and the row index.

**Reading data.** `read_csv` from a file or URL, first looks with `info`, `describe` and `value_counts`, and telling pandas what it can't guess, like codes with leading zeros.

**Selecting.** `loc` vs `iloc` (and why `.loc[0:2]` includes 2), boolean filters, `isin` and `query`.

**Modifying safely.** The single-step `.loc[rows, col] = value` rule, why chained assignment fails, `.copy()` for subsets, and how missing values fall through conditions.

**Missing values.** `isna`, `dropna(subset=...)`, filling with training-only statistics, and why pandas and NumPy disagree about `mean`.

**Transforming columns.** Vectorized arithmetic, `assign` chains, `np.where`, `map`, `.str` and `.dt`.

**groupby.** Named aggregation, `transform` for per-row group statistics, `crosstab` and `pivot_table`.

**Joining tables.** `merge` and its `how` options, `indicator`, the duplicate key trap and `validate`, and `concat`.

**Categories and one-hot encoding.** `get_dummies`, and aligning test columns to the training columns.

**Performance.** `iterrows` vs `apply` vs vectorized, measured.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `load_scores`, `missing_summary`, `filter_and_sort`, `add_features` |
| Medium | `species_table`, `fill_by_group`, `zscore_within_group`, `one_hot_aligned`, `top_n_per_group` |
| Hard | `enrich_orders`, `rfm_features`, `monthly_pivot` |

The checks are built to catch silent mistakes: IDs that lose their leading zeros, functions that modify the caller's DataFrame, fill values that leak test data, test sets with different columns from training, joins that drop or duplicate rows, features that use orders from the future, and months that vanish from a pivot table.

## Mini project

A shop's customers, products, orders and order items, simulated as four related tables. They're joined with validated merges, then turned into one row per customer: RFM features, activity trends, basket statistics and profile, all computed strictly from data before a cutoff date. Logistic regression (NumPy, from Module 6) is compared against a majority baseline and a simple "no order in 60 days" rule.

The model ties the rule at 87.4% accuracy, and the notebook reports that honestly. Recency is that strong a signal. It explains what a model still adds over the rule. Then it shows the trap the project is really about: building features without the cutoff produces a 99.6% accurate model that's completely useless, because the sign of one feature is effectively the label.

## Prerequisites

Modules [1](../01-python-foundations) to [6](../06-numpy), especially [Module 5](../05-files-and-data) (the penguin pipeline) and [Module 6](../06-numpy) (NumPy).

## Next

Module 8: Visualization (coming soon)
