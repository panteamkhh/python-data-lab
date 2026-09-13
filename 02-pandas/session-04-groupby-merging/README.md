# 📘 Session 4 — GroupBy & Merging

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">🐼 Pandas Track</a> ·
  <a href="../session-03-cleaning-data/README.md">⬅️ Session 3</a> ·
  <a href="../session-05-time-series-and-io/README.md">Session 5 ➡️</a>
</p>

> **Goal:** split data into groups, summarize each group with `agg` and `transform`, reshape with `value_counts` / `crosstab` / `pivot_table`, and combine tables with `merge`, `join`, and `concat`.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Explain the **split–apply–combine** pattern that powers `groupby`.
- Aggregate one or many columns with `.agg()`, including **named aggregation** such as `.agg(total=("revenue", "sum"))`.
- Add group-level values back onto every row with `.transform()`.
- Count and cross-tabulate categories with `value_counts` and `crosstab`.
- Reshape long data into a summary matrix with `pivot_table`.
- Combine DataFrames using `merge` / `join` (all four `how=` modes) and stack them with `concat`.

---

## 🧠 Concept Explanation

### 4.1 The split–apply–combine idea

**Intuition first.** Picture a stack of sales receipts. To find total sales per region, you would sort the receipts into piles — one pile per region — add up each pile, then write the totals on a whiteboard. That is exactly what `groupby` does: it **splits** rows by a key, **applies** an operation to each group, and **combines** the results into a new table.

**Formal explanation.** `DataFrame.groupby(key)` returns a **GroupBy object**. It does not compute anything yet — it simply records how the data should be split. Calling an aggregation method (`.sum()`, `.mean()`, `.count()`, `.agg(...)`) triggers the apply and combine steps.

```python
import pandas as pd

sales = pd.DataFrame({
    "region":  ["North", "South", "North", "South", "North", "South"],
    "product": ["Laptop", "Phone", "Phone", "Laptop", "Tablet", "Tablet"],
    "revenue": [1200, 800, 400, 1500, 500, 900],
})

sales.groupby("region")["revenue"].sum()
```

The result is a `Series` indexed by `region`. Group by two keys instead — `sales.groupby(["region", "product"])["revenue"].sum()` — and the result is indexed by both, i.e. a `MultiIndex`.

### 4.2 Aggregating with `.agg()`

`.sum()` and `.mean()` are convenient shortcuts. When you need more than one statistic, reach for `.agg()`:

- Pass a **list** of function names to get one output column per statistic:

  ```python
  sales.groupby("region")["revenue"].agg(["sum", "mean", "max"])
  ```

- Pass a **dictionary** to choose different statistics per column:

  ```python
  sales.groupby("region").agg({"revenue": ["sum", "mean"], "product": "count"})
  ```

- Use **named aggregation** to name each output explicitly and avoid MultiIndex columns:

  ```python
  sales.groupby("region").agg(
      total=("revenue", "sum"),
      avg=("revenue", "mean"),
      orders=("product", "count"),
  )
  ```

The left side of each pair is the **new column name**; the right side is a `(column, function)` tuple. Named aggregation is the most readable form and the recommended style in pandas 3.x.

### 4.3 Broadcasting group results with `.transform()`

`agg` **collapses** each group into a single row. `transform` instead returns a result with the **same index and length as the original DataFrame**, so a group-level value can be attached back to every row.

```python
sales["region_total"] = sales.groupby("region")["revenue"].transform("sum")
sales["share"] = sales["revenue"] / sales["region_total"]
```

This is the idiomatic way to compute "each sale as a share of its region total" without a merge. Other common uses: group z-scores, group ranks, and filling missing values with the group mean.

### 4.4 Counting categories: `value_counts`

`Series.value_counts()` counts how often each unique value appears:

```python
sales["region"].value_counts()                 # counts, most frequent first
sales["region"].value_counts(normalize=True)   # proportions instead of counts
sales["region"].value_counts(dropna=False)     # include missing labels
```

By default the result is sorted from most to least frequent. `normalize=True` switches from counts to proportions.

### 4.5 Cross-tabulation: `crosstab`

`pd.crosstab` counts combinations of two (or more) categorical columns:

```python
pd.crosstab(sales["region"], sales["product"])
pd.crosstab(sales["region"], sales["product"], margins=True)
```

`margins=True` appends row and column totals. To aggregate a **numeric value** instead of counting rows, pass `values=` and `aggfunc=`:

```python
pd.crosstab(
    sales["region"], sales["product"],
    values=sales["revenue"], aggfunc="sum",
)
```

### 4.6 Reshaping with `pivot_table`

`pivot_table` is the spreadsheet-style "pivot": one column becomes the rows, another becomes the columns, and a value column is aggregated into the cells.

```python
sales.pivot_table(
    values="revenue",
    index="region",
    columns="product",
    aggfunc="sum",
    fill_value=0,
    margins=True,
)
```

Missing combinations appear as `NaN` unless you set `fill_value`. Use `aggfunc="mean"` (the default) to compare averages instead of totals. `pivot_table` is built on top of `groupby`, so anything you can group you can pivot.

### 4.7 Combining tables with `merge` (SQL-style)

Real data lives in several tables. `merge` joins them on one or more key columns — exactly like a SQL `JOIN`.

```python
customers = pd.DataFrame({
    "customer_id": [1, 2, 3],
    "name": ["Alice", "Bob", "Carol"],
    "region": ["North", "South", "East"],
})

orders = pd.DataFrame({
    "order_id": [10, 11, 12],
    "customer_id": [1, 2, 4],
    "revenue": [250, 400, 150],
})

orders.merge(customers, on="customer_id", how="left")
```

The `how` argument decides which keys survive:

| `how=` | Keeps rows from… | Missing side becomes |
|---|---|---|
| `"inner"` | only keys present in **both** frames | *(rows dropped)* |
| `"left"` | all keys from the **left** frame | `NaN` |
| `"right"` | all keys from the **right** frame | `NaN` |
| `"outer"` | all keys from **either** frame | `NaN` |

Useful extras:

- `suffixes=("_orders", "_customers")` disambiguates overlapping non-key columns.
- `validate="many_to_one"` raises an error when the relationship is not what you expect — a great safety net.
- Different key names? Use `left_on="customer_id", right_on="cust_id"`.

### 4.8 `join` and `concat`

`DataFrame.join` is a convenience wrapper around `merge` that joins **on the index**:

```python
by_region = sales.groupby("region")[["revenue"]].sum()
targets = pd.DataFrame(
    {"target": [2000, 1500, 1000]},
    index=["North", "South", "East"],
)
by_region.join(targets, how="left")
```

`pd.concat` **stacks** DataFrames instead of matching keys:

```python
pd.concat([q1, q2], ignore_index=True)   # stack rows (axis=0, the default)
pd.concat([features, labels], axis=1)    # glue columns side by side
pd.concat({"first": q1, "second": q2})   # add an extra outer key level
```

Rule of thumb: use **`merge` / `join`** to combine tables by a shared key; use **`concat`** to stack tables that already share the same columns (or index).

---

## ⚙️ Why This Matters

- **Reporting & dashboards:** "sales by region", "signups per month", and "errors per service" are each one `groupby().agg()` away.
- **Feature engineering:** `transform` adds group-aware features — share of total, group average, group rank — that improve machine-learning models.
- **Data integration:** real datasets are split across tables (orders, customers, products). `merge` is how you assemble them into a single analysis table.
- **Reshaping for readability:** `pivot_table` and `crosstab` turn long, hard-to-scan logs into compact summary matrices a human can read in seconds.
- **Cleaning pipelines:** `concat` is how you rebuild one clean table from many monthly exports or shards.

---

## 🔍 Key Insights

- `groupby` is **lazy**: the split happens when you call an aggregation, not when you create the GroupBy object.
- `agg` **reduces** rows (one per group); `transform` **preserves** rows (same length as the input). Choosing the wrong one is a classic beginner mistake.
- Named aggregation `agg(total=("revenue", "sum"))` produces clean column names; passing a list or dict often yields MultiIndex columns that are harder to use downstream.
- `value_counts` and `crosstab` answer related but different questions: `value_counts` counts one column; `crosstab` counts the **combination** of two or more.
- `pivot_table` drops or fills missing combinations — always decide whether `NaN` or `0` is the honest answer (`fill_value=0` can hide genuinely absent data).
- `merge` is SQL `JOIN`; `concat` is "glue". Confusing them leads to exploded row counts. Use `validate=` to catch surprises.
- An **inner** join silently drops unmatched keys; a **left** join keeps them with `NaN`. Always compare row counts before and after a join.

---

## 📦 Summary

`groupby` implements split–apply–combine: split rows by a key, apply an operation, and combine the result. Use `.agg()` — ideally **named aggregation** — to compute several statistics at once, and `.transform()` to broadcast a group result back onto every row. `value_counts`, `crosstab`, and `pivot_table` summarize and reshape categorical data into counts, cross-tabulations, and matrix-style summaries. To combine tables, `merge` (and its index-based sibling `join`) match rows by key using `how="inner"`, `"left"`, `"right"`, or `"outer"`, while `concat` stacks DataFrames along rows or columns. Together these tools turn many raw tables into one tidy, summarized analysis table.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Group by one key | `df.groupby("region")["revenue"].sum()` |
| Group by two keys | `df.groupby(["region", "product"])["revenue"].sum()` |
| Several statistics | `df.groupby("region")["revenue"].agg(["sum", "mean"])` |
| Named aggregation | `df.groupby("region").agg(total=("revenue", "sum"))` |
| Group value on each row | `df.groupby("region")["revenue"].transform("sum")` |
| Count categories | `df["region"].value_counts(normalize=True)` |
| Cross-tab counts | `pd.crosstab(df["region"], df["product"], margins=True)` |
| Pivot / summarize | `df.pivot_table(values="revenue", index="region", columns="product", aggfunc="sum", fill_value=0)` |
| SQL-style join | `left.merge(right, on="id", how="left")` |
| Index join | `left.join(right, how="left")` |
| Stack rows / columns | `pd.concat([a, b], ignore_index=True)` / `pd.concat([a, b], axis=1)` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-03-cleaning-data/README.md">⬅️ Session 3</a> ·
  <a href="../session-05-time-series-and-io/README.md">Session 5 ➡️</a>
</p>
