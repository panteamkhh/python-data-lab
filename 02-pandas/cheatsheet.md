# 📄 Pandas Cheat Sheet

A one-page reference for everyday pandas. Every snippet assumes:

```python
import pandas as pd
import numpy as np
```

---

## 1. Creating data

```python
pd.Series([10, 20, 30], index=["a", "b", "c"], name="score")

df = pd.DataFrame({
    "name": ["Ava", "Ben", "Cleo"],
    "score": [88, 92, 79],
    "city": ["Tehran", "Berlin", "Rome"],
})

pd.DataFrame([{"a": 1, "b": 2}, {"a": 3, "b": 4}])   # list of dicts
pd.read_csv("data.csv")                              # from a file
pd.read_csv("data.csv", parse_dates=["date"])
```

## 2. Inspecting

```python
df.head(5)          # first rows
df.tail(3)          # last rows
df.shape            # (rows, cols)
df.info()           # dtypes + non-null counts
df.describe()       # numeric summary
df.dtypes           # column types
df.columns          # column labels
df.index            # row labels
df.nunique()        # distinct values per column
df["score"].value_counts()   # frequency of each value
```

## 3. Selecting: `loc` vs `iloc`

```python
df["score"]              # one column -> Series
df[["name", "score"]]    # several columns -> DataFrame

df.loc[0, "score"]       # by LABEL
df.loc[0:2, "name":"score"]  # label slices are INCLUSIVE
df.iloc[0, 1]            # by POSITION (row, col)
df.iloc[0:2, 0:2]        # positional slices are EXCLUSIVE

df.at[0, "name"]         # fast scalar by label
df.iat[0, 0]             # fast scalar by position
```

## 4. Filtering

```python
df[df["score"] > 85]
df[(df["score"] > 80) & (df["city"] == "Berlin")]   # parentheses REQUIRED
df[df["city"].isin(["Rome", "Berlin"])]
df[df["score"].between(80, 95)]
df.query("score > 85 and city == 'Berlin'")
df.sort_values("score", ascending=False)
df.nlargest(3, "score")
```

## 5. Creating / updating columns

```python
df["passed"] = df["score"] >= 80
df["bonus"] = df["score"] * 1.1
df.loc[df["score"] < 80, "grade"] = "F"     # conditional assign with .loc
df.rename(columns={"score": "points"})
df.drop(columns=["city"])
```

## 6. Missing data

```python
df.isna()              # mask of missing values
df.isna().sum()        # missing count per column
df.dropna()            # drop rows with any NaN
df.dropna(subset=["score"])
df.fillna(0)
df["score"].fillna(df["score"].mean())
```

## 7. Types & strings

```python
df["score"].astype(float)
pd.to_numeric(df["score"], errors="coerce")
pd.to_datetime(df["date"])
df["city"] = df["city"].astype("category")

df["name"].str.lower()
df["name"].str.strip()
df["name"].str.contains("a", case=False)
df["email"].str.split("@").str[0]
```

## 8. Duplicates

```python
df.duplicated()                 # boolean mask
df.drop_duplicates()
df.drop_duplicates(subset=["name"])
```

## 9. GroupBy & aggregation (split–apply–combine)

```python
df.groupby("city")["score"].mean()
df.groupby("city").agg(
    avg_score=("score", "mean"),
    count=("score", "size"),
    max_score=("score", "max"),
)
df.groupby("city")["score"].transform("mean")   # align result back to rows
```

## 10. Reshaping

```python
pd.crosstab(df["city"], df["passed"])
df.pivot_table(index="city", columns="passed", values="score", aggfunc="mean")
df.pivot_table(index="city", values="score", aggfunc=["mean", "sum"])
```

## 11. Combining tables

```python
pd.concat([df1, df2], axis=0)                 # stack rows
pd.concat([df1, df2], axis=1)                 # stack columns side by side
df1.merge(df2, on="id", how="inner")          # also: left / right / outer
df1.merge(df2, left_on="cust_id", right_on="id")
```

## 12. Time series

```python
df["date"] = pd.to_datetime(df["date"])
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df = df.set_index("date").sort_index()

df.resample("M")["sales"].sum()        # monthly totals (M, W, D, Q, Y)
df["sales"].rolling(7).mean()          # 7-period moving average
df["sales"].shift(1)                   # previous value
df["sales"].diff()                     # change from previous
df["sales"].pct_change()               # relative change
```

## 13. Saving

```python
df.to_csv("out.csv", index=False)
df.to_excel("out.xlsx", index=False)   # needs openpyxl
df.to_parquet("out.parquet")           # needs pyarrow
```

---

## ⚠️ Top pitfalls

1. **`loc` is inclusive, `iloc` is exclusive** on slices.
2. Combine conditions with `&`/`|` and **parentheses**: `(a) & (b)`.
3. In pandas 3.x **Copy-on-Write is default** — never chain assignment; use `.loc`.
4. A CSV round-trip turns `int` into `float64`; re-cast with `.astype(int)`.
5. `groupby` returns an index; add `.reset_index()` to flatten it back to columns.
6. `apply` is a last resort — prefer vectorized ops, `.str`, `.dt`, and `groupby.agg`.

---

<div align="center"><a href="./README.md">🏠 Back to the track</a> · <a href="../README.md">🧪 Lab home</a></div>
