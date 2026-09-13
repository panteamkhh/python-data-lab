# 📘 Session 3 — Cleaning Data

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">🐼 Pandas Track</a> ·
  <a href="../session-02-indexing-selecting-filtering/README.md">⬅️ Session 2</a> ·
  <a href="../session-04-groupby-merging/README.md">Session 4 ➡️</a>
</p>

> **Goal:** turn a messy CSV into a tidy DataFrame by handling missing values, fixing dtypes, removing duplicates, and cleaning text — all Copy-on-Write-safely.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Detect missing values with `isna` / `notna` and summarize them per column.
- Remove or fill missing values with `dropna` and `fillna`.
- Convert column types using `astype`, `to_numeric`, and `to_datetime`, and use the `category` dtype.
- Detect and remove duplicate rows with `duplicated` and `drop_duplicates`.
- Clean text columns with the `.str` accessor.
- Rename columns and replace values with `rename` and `replace`.

---

## 🧠 Concept Explanation

### 3.1 Why cleaning is the real job

**Intuition first.** A raw CSV is like a box of receipts dumped on a desk: some rows are missing a field, some numbers were typed as text, some receipts are duplicates, and the city names are spelled inconsistently. Before you can analyse anything, you tidy the pile. Data cleaning is often 60–80% of a real project.

**Formal explanation.** pandas represents missing values as `NaN` (for floats) or `pd.NA` / `NaT` (for nullable and datetime types). Cleaning means making three things consistent: **missingness** (what to do with absent values), **dtypes** (each column in the right type), and **duplicates/text** (one canonical version of each record).

### 3.2 Finding missing data

`isna()` returns a boolean DataFrame that is `True` wherever a value is missing; `notna()` is its inverse.

```python
df.isna()              # element-wise True/False
df.isna().sum()        # count of missing values per column
df.notna().all()       # which columns are fully populated
```

> **Under the hood:** integers cannot store `NaN`, so a column with any missing integer values becomes `float64` (or a nullable `Int64`). This is a common, surprising dtype change to watch for.

### 3.3 Removing or filling missing values

- **`dropna()`** removes rows (or columns) containing missing values.
  - `dropna()` drops any row with at least one `NaN`.
  - `dropna(subset=["price"])` only drops rows missing `price`.
  - `dropna(how="all")` drops rows where *everything* is missing.
  - `dropna(axis=1)` drops columns with missing values.
- **`fillna(value)`** replaces missing values.
  - `fillna(0)` — a constant.
  - `fillna(df["price"].mean())` — the column mean.
  - `fillna(method="ffill")` is deprecated in pandas 3.x; use **`ffill()` / `bfill()`** instead.

```python
df = df.dropna(subset=["price"])
df["units"] = df["units"].fillna(0)
df["price"] = df["price"].ffill()
```

> **CoW reminder:** `dropna` and `fillna` **return** a new object. Assign the result (or reassign the column) — do not rely on `inplace=True`.

### 3.4 Converting dtypes

Real CSVs mix types: a `"price"` column may contain `"12.5"`, `"7.5"`, and `"n/a"`, so pandas loads it as text (`str`). To fix it:

- **`astype("float64")`** — strict cast; fails on unparseable entries.
- **`to_numeric(series, errors="coerce")`** — turns unparseable entries into `NaN` instead of raising. The safest first step for messy numbers.
- **`to_datetime(series, errors="coerce")`** — parses date-like text into `datetime64`; again, `coerce` protects against bad values.
- **`astype("category")`** — stores a column with a small set of repeated labels (like `"city"`) efficiently, as codes plus a lookup table.

```python
import numpy as np
import pandas as pd

df["price"] = pd.to_numeric(df["price"], errors="coerce")
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df["city"] = df["city"].astype("category")
```

### 3.5 Duplicates

- **`duplicated()`** returns a boolean mask marking rows that already appeared earlier.
- **`drop_duplicates()`** removes them.
- Both accept `subset=[...]` to consider only certain columns, and `keep="first"` / `"last"` / `False` to control which copy survives.

```python
df.duplicated().sum()                              # how many duplicates
df = df.drop_duplicates()                          # remove exact duplicates
df = df.drop_duplicates(subset=["email"], keep="last")
```

### 3.6 String methods with `.str`

The `.str` accessor applies vectorized string operations to a whole text column — no Python loop needed.

```python
df["name"] = df["name"].str.strip()                 # remove surrounding spaces
df["city"] = df["city"].str.title()                 # "berlin" -> "Berlin"
df["email"] = df["email"].str.lower()               # normalize case
df["code"] = df["code"].str.replace("-", "")        # remove a character
df["len"] = df["name"].str.len()                    # string length
df[df["email"].str.contains("@", na=False)]         # substring test
df["name"].str.split(" ", n=1, expand=True)         # split into columns
```

Pass `na=False` to `contains`/`startswith` when the column has missing values. On object-dtype columns the default yields `NaN` for missing entries, which cannot be used as a boolean mask; pandas 3.x's `str` dtype returns `False` instead. Being explicit gives you the same predictable result either way.

### 3.7 Renaming and replacing

- **`rename(columns={...})`** changes column labels; `rename(index={...})` changes row labels.
- **`replace({old: new})`** swaps values throughout a column or frame.

```python
df = df.rename(columns={"cust_name": "customer", "qty": "units"})
df["status"] = df["status"].replace({"ok": "active", "n/a": "unknown"})
```

Both return new objects; assign the result.

---

## ⚙️ Why This Matters

- **Garbage in, garbage out.** Every model or chart is only as trustworthy as the data beneath it; cleaning is what makes analysis valid.
- **Wrong dtypes cause silent failures.** A numeric column typed as text breaks arithmetic, sorting, and plotting in confusing ways.
- **Missing data is a decision.** Dropping vs. filling changes your results — being deliberate (and knowing the trade-off) is a core analyst skill.
- **Duplicates and text variance inflate counts.** Without deduplication and normalization, totals are simply wrong.

---

## 🔍 Key Insights

- `isna().sum()` is the fastest way to see the shape of missingness before deciding what to do.
- `to_numeric(..., errors="coerce")` is the safe default for messy numeric text — it converts problems into visible `NaN`s instead of crashing.
- `category` dtype shrinks memory and speeds up grouping when a column has few unique values.
- `.str` methods are vectorized and skip missing values; boolean ones should pass `na=False` explicitly.
- `dropna`, `fillna`, `drop_duplicates`, `rename`, and `replace` all **return new objects** — assign the result under Copy-on-Write.
- `fillna(method=...)` is a pandas 2.x habit; in pandas 3.x use `.ffill()` and `.bfill()`.

---

## 📦 Summary

Cleaning a DataFrame means making missingness, dtypes, duplicates, and text consistent. Use `isna`/`notna` to locate missing values, then `dropna` or `fillna` (with `.ffill()`/`.bfill()` for propagation) to handle them. Convert types with `astype`, `to_numeric(errors="coerce")`, `to_datetime(errors="coerce")`, and `astype("category")`. Remove repeated rows with `duplicated`/`drop_duplicates`. Normalize text via `.str` (`strip`, `title`, `lower`, `replace`, `contains`), and tidy labels and values with `rename` and `replace`. Because all of these return new objects, always assign the result — the Copy-on-Write-safe habit.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Count missing | `df.isna().sum()` |
| Present values | `df.notna().all()` |
| Drop rows with NaN | `df = df.dropna(subset=["price"])` |
| Drop fully-empty rows | `df = df.dropna(how="all")` |
| Fill with constant | `df["units"] = df["units"].fillna(0)` |
| Fill with mean | `df["p"] = df["p"].fillna(df["p"].mean())` |
| Forward / backward fill | `df["p"].ffill()`, `df["p"].bfill()` |
| Safe numeric cast | `pd.to_numeric(df["p"], errors="coerce")` |
| Parse dates | `pd.to_datetime(df["d"], errors="coerce")` |
| Categorical column | `df["city"] = df["city"].astype("category")` |
| Count duplicates | `df.duplicated().sum()` |
| Drop duplicates | `df = df.drop_duplicates(subset=["email"])` |
| Strip / case text | `df["n"].str.strip()`, `df["c"].str.title()` |
| Substring mask | `df[df["e"].str.contains("@", na=False)]` |
| Rename columns | `df = df.rename(columns={"qty": "units"})` |
| Replace values | `df["s"] = df["s"].replace({"ok": "active"})` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-02-indexing-selecting-filtering/README.md">⬅️ Session 2</a> ·
  <a href="../session-04-groupby-merging/README.md">Session 4 ➡️</a>
</p>
