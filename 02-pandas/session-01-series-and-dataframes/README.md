# 📘 Session 1 — Series & DataFrames

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">🐼 Pandas Track</a> ·
  <a href="../session-02-indexing-selecting-filtering/README.md">Session 2 ➡️</a>
</p>

> **Goal:** create pandas `Series` and `DataFrame`s from scratch, inspect them confidently, and select or derive columns — no external data files needed.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Explain what a pandas `Series` is and build one from a list or a dictionary.
- Give a `Series` a custom index and a `.name`.
- Build a `DataFrame` from a dict of lists, a list of dicts, and CSV text via `io.StringIO`.
- Inspect a `DataFrame` with `head`, `tail`, `info`, `describe`, `.shape`, `.dtypes`, `.columns`, and `.index`.
- Select a single column (`df['a']`) and multiple columns (`df[['a', 'b']]`).
- Add and derive new columns safely under pandas' Copy-on-Write behaviour.

---

## 🧠 Concept Explanation

### 1.1 Why pandas? The table idea

**Intuition first.** Think of a spreadsheet. It has named columns, numbered rows, and cells that hold values. You can sort it, filter it, and add a new calculated column in one go. pandas gives you that same spreadsheet object inside Python — except it is scriptable, reproducible, and handles millions of rows.

**Formal explanation.** pandas is a Python library for **labeled tabular data**. Its two core objects are the **`Series`** (a labeled one-dimensional array) and the **`DataFrame`** (a labeled two-dimensional table, i.e., a collection of `Series` sharing one index). pandas is built on top of NumPy, so columns of numbers are stored efficiently, but pandas adds **labels** (row and column names) and a huge toolbox for wrangling real-world data.

> **Convention:** we write `import pandas as pd` everywhere. The `pd` alias is used across the entire pandas community.

**Why it matters.** NumPy is perfect for raw numeric arrays but painful for mixed, labeled data (names next to numbers next to dates). pandas is the standard tool for loading, cleaning, exploring, and reshaping that kind of data — which is most of data science.

### 1.2 The `Series`: a labeled 1D array

A `Series` is like a NumPy array plus an **index** (the labels for each element). Under the hood it *is* built on a NumPy array.

- Created from a **list**, the index is the default `RangeIndex` `0, 1, 2, …`.
- Created from a **dict**, the dict **keys become the index** and the values become the data.
- Passing `index=` lets you attach your own labels.
- `.name` stores a label for the whole Series, which becomes useful when it is placed inside a DataFrame.

```python
import pandas as pd

temps = pd.Series([21.5, 23.0, 19.8], index=["Mon", "Tue", "Wed"], name="temp_c")
temps
```

### 1.3 The `DataFrame`: a labeled 2D table

A `DataFrame` is a table with **rows** and **columns**. Each column is a `Series`, and all columns share the same row index. You can think of it as a dict of `Series` with a common index.

It has three pieces of metadata you will read constantly:

- **`.index`** — the row labels.
- **`.columns`** — the column labels.
- **`.dtypes`** — the data type of each column (one dtype per column, because each column is a homogeneous Series).

### 1.4 Building a DataFrame three ways

**(a) From a dict of lists** — the most common construction. Each key is a column name and each list is that column's values. All lists must have the same length.

```python
df = pd.DataFrame({"name": ["Ana", "Bo"], "score": [88, 92]})
```

**(b) From a list of dicts** — one dict per row. pandas unions the keys to form the columns, filling missing keys with `NaN`.

```python
rows = [{"name": "Ana", "score": 88}, {"name": "Bo", "score": 92}]
df = pd.DataFrame(rows)
```

**(c) From CSV text via `io.StringIO`** — this mirrors `pd.read_csv` from a file, but keeps the example **self-contained** with no external files. `io.StringIO` wraps a Python string so pandas can read it like a file object.

```python
import io

csv = "name,score\nAna,88\nBo,92\n"
df = pd.read_csv(io.StringIO(csv))
```

> **Why `StringIO`?** It teaches the exact `read_csv` workflow (headers, parsing, dtypes) while guaranteeing the notebook runs anywhere, offline.

### 1.5 Inspecting a DataFrame

Once you have a DataFrame, these are the first calls you make on any unfamiliar dataset:

| Call | What it gives you |
|---|---|
| `df.head(n)` / `df.tail(n)` | First / last `n` rows (default 5) |
| `df.shape` | Tuple `(rows, columns)` |
| `df.columns` | Column labels |
| `df.index` | Row labels |
| `df.dtypes` | Data type of each column |
| `df.info()` | Non-null counts, dtypes, and memory usage |
| `df.describe()` | Count, mean, std, min, quartiles, max for numeric columns |

`describe()` only summarizes numeric columns by default. Passing `include="all"` adds object/categorical columns, where the statistics switch to `count`, `unique`, `top`, and `freq`.

```python
df.shape          # (2, 2)
df.dtypes         # name: str, score: int64
df.describe()     # summary statistics
```

### 1.6 Selecting and adding columns

- **Single column:** `df["score"]` returns a `Series`.
- **Multiple columns:** `df[["name", "score"]]` returns a new `DataFrame`. Note the **double brackets** — the outer brackets are selection, the inner list is the set of names.

To **add** a column, assign a Series or a scalar to a new name:

```python
df["passed"] = df["score"] >= 60          # element-wise comparison
df["score_curve"] = df["score"] + 5       # arithmetic on a whole column
```

If you assign a `Series`, pandas aligns it **by index** — another reminder that labels, not positions, drive pandas.

### 1.7 Assignment and Copy-on-Write

Since pandas 3.0, **Copy-on-Write (CoW) is the default**. That means any index/column selection returns a lazy view, and the moment you try to *write* through it, pandas makes a copy so the original is never silently mutated.

Two practical consequences:

1. **Never** use chained assignment like `df["x"][0] = 5`. Under CoW it either raises a warning or modifies a temporary copy — never the parent. Use `.loc` instead: `df.loc[0, "x"] = 5`.
2. Reassigning a column (`df["score"] = df["score"].fillna(0)`) is the idiomatic, CoW-safe way to transform data. Avoid relying on `inplace=True`.

```python
df.loc[0, "score"] = 95          # correct, explicit, CoW-safe
df["score"] = df["score"] + 1    # correct column reassignment
```

---

## ⚙️ Why This Matters

- **Every dataset starts as a table.** CSV exports, SQL results, spreadsheets, and API responses all land in pandas as a `DataFrame` — inspection is your first move on any of them.
- **Labels beat positions.** Real data has meaningful row identifiers (dates, IDs) and column names. pandas' index/columns model is what makes joining, grouping, and time-series work possible later.
- **Exploration is a skill.** Knowing `info()` and `describe()` cold lets you spot wrong dtypes and missing data before they cause silent bugs.
- **Copy-on-Write changes old habits.** Most outdated pandas tutorials teach chained assignment; knowing the CoW-safe patterns keeps your code correct and future-proof.

---

## 🔍 Key Insights

- A `DataFrame` is a dict of `Series` sharing one index; each column has a single dtype.
- Build Series/DataFrames from **dicts** when you want keys to become labels.
- `df["a"]` → `Series`; `df[["a"]]` → `DataFrame`. The bracket count is not a typo.
- `io.StringIO` lets you practice `read_csv` without shipping a data file.
- `describe()` silently ignores non-numeric columns unless you pass `include`.
- Under Copy-on-Write, **chained assignment never updates the parent** — always use `df.loc[row, col] = value`.

---

## 📦 Summary

pandas stores labeled tabular data in two objects: `Series` (1D, labeled) and `DataFrame` (2D table of aligned `Series`). You can build them from lists, dicts, lists of dicts, or CSV text read through `io.StringIO`. Inspection methods — `head`, `tail`, `info`, `describe`, `.shape`, `.dtypes`, `.columns`, `.index` — tell you everything you need before analysing. Columns are selected with `df["a"]` or `df[["a", "b"]]`, and new columns are created by assignment. pandas 3.0's Copy-on-Write default means you assign through `.loc` and reassign columns explicitly rather than relying on chained assignment or `inplace=True`.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Alias pandas | `import pandas as pd` |
| Series from list | `pd.Series([10, 20, 30])` |
| Series from dict | `pd.Series({"a": 1, "b": 2})` |
| Series with labels | `pd.Series(data, index=[...], name="x")` |
| DataFrame from dict of lists | `pd.DataFrame({"a": [1, 2], "b": [3, 4]})` |
| DataFrame from list of dicts | `pd.DataFrame([{"a": 1}, {"a": 2}])` |
| DataFrame from CSV text | `pd.read_csv(io.StringIO(csv))` |
| Preview rows | `df.head()`, `df.tail()` |
| Structure & dtypes | `df.info()`, `df.dtypes`, `df.shape` |
| Labels | `df.columns`, `df.index` |
| Summary stats | `df.describe()`, `df.describe(include="all")` |
| One column / many columns | `df["a"]` / `df[["a", "b"]]` |
| Add a derived column | `df["c"] = df["a"] + df["b"]` |
| Safe cell assignment | `df.loc[0, "a"] = 99` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-02-indexing-selecting-filtering/README.md">Session 2 ➡️</a>
</p>
