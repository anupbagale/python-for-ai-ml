# Module 7 Cheatsheet

## Creating and inspecting

```python
import pandas as pd

df = pd.DataFrame({"a": [1, 2], "b": ["x", "y"]})
s = pd.Series([1.0, 2.0], index=["p1", "p2"], name="mass")

df.head(), df.info(), df.describe(), df.shape, df.dtypes
df["col"].value_counts(normalize=True)
df.to_numpy()                                  # hand off to NumPy
```

## Reading and writing

```python
pd.read_csv(path_or_url,
            na_values=["?", "-"],              # extra missing markers
            dtype={"zip": str},                # keep leading zeros
            parse_dates=["date"],
            encoding="utf-8-sig",              # Excel BOM
            usecols=[...], nrows=1000)
df.to_csv(path, index=False)                   # index=False avoids an extra column
```

## Selecting

```python
df["a"]                    # Series
df[["a", "b"]]             # DataFrame
df.loc[0, "a"]             # by label; .loc[0:2] INCLUDES 2
df.iloc[0, 0]              # by position; .iloc[0:2] excludes 2
df[(df.a > 1) & (df.b == "x")]
df[df.col.isin(["x", "y"])]
df.query("a > 1 and b == 'x'")
df.loc[mask, ["a", "b"]]   # rows and columns in one step
```

## Modifying safely

```python
df.loc[mask, "col"] = value          # one step: always works
# df[mask]["col"] = value            # chained: silently does nothing
sub = df[mask].copy()                # copy subsets you'll modify
new = df.assign(c=lambda d: d.a * 2) # returns a new DataFrame
```

`NaN` fails every comparison, so rows with missing values fall through conditions. Check that categories add up.

## Missing values

```python
df.isna().sum()
df.isna().any(axis=1).sum()          # rows with any missing value
df.dropna(subset=["a", "b"])
df.fillna({"sex": "unknown"})
medians = train[num_cols].median()
train, test = train.fillna(medians), test.fillna(medians)   # train statistics only
```

pandas skips `NaN` in `mean`/`sum`; NumPy doesn't.

## Transforming

```python
df["ratio"] = df.a / df.b
np.where(df.a > 1, "big", "small")
df.col.map({"x": 0, "y": 1})                 # unmapped values become NaN
df.col.str.strip().str.lower().str.replace("x", "y", regex=False)
pd.to_datetime(df.date)
df.date.dt.month, df.date.dt.day_name(), df.date.dt.to_period("M")
(df.end - df.start).dt.days
```

## groupby

```python
df.groupby("g")["x"].mean()
df.groupby("g").agg(n=("x", "size"), mean_x=("x", "mean"), max_y=("y", "max"))
df.groupby(["g", "h"]).size()
df["g_mean"] = df.groupby("g")["x"].transform("mean")    # one value per row
df.groupby("g").sample(frac=0.2, random_state=0)          # stratified sample
df.groupby("g").head(3)
pd.crosstab(df.a, df.b)
df.pivot_table(index="a", columns="b", values="x", aggfunc="mean", fill_value=0)
```

`std` defaults to `ddof=1` in pandas (NumPy's default is 0).

## Joining

```python
orders.merge(customers, on="customer_id", how="left",
             validate="many_to_one",       # catch duplicate keys
             indicator=True)               # adds _merge: left_only / right_only / both
pd.concat([a, b], ignore_index=True)
```

The default `how="inner"` silently drops unmatched rows. Duplicate keys multiply rows.

## Encoding

```python
train_enc = pd.get_dummies(train, columns=["cat"], dtype=int)
test_enc = pd.get_dummies(test, columns=["cat"], dtype=int).reindex(columns=train_enc.columns, fill_value=0)
df.cat.astype("category").cat.codes
```

## Reindexing to a complete set

```python
counts.reindex(all_ids, fill_value=0)                         # entities with no rows
table.reindex(columns=pd.period_range(start, end, freq="M"), fill_value=0)   # missing months
```

## Sorting and tidying

```python
df.sort_values(["g", "x"], ascending=[True, False], kind="stable")
df.reset_index(drop=True)
df.drop(index=other.index), df.drop(columns="c")
df.drop_duplicates(subset=["id"])
df.rename(columns={"old": "new"})
```

## Performance

Vectorized column operations, then `.map`/`.str`/`.dt`/`np.where`, then `groupby`. `apply(axis=1)` and `iterrows` are last resorts: often 100 to 1000 times slower.

## Leakage checklist (time-based data)

- Pick an explicit cutoff. Features use only rows before it; labels use only rows after it.
- Pass the feature function only the `past` table.
- Compute fill values, scaling statistics and encodings from training rows only.
- If results look too good, suspect leakage first.
