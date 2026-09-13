# 🐼 Pandas Track

<p align="center">
  <a href="../README.md">🏠 Lab Home</a> ·
  <a href="./cheatsheet.md">📄 Pandas Cheat Sheet</a> ·
  <a href="../01-numpy/README.md">⬅️ NumPy Track</a> ·
  <a href="../03-matplotlib/README.md">📊 Matplotlib Track ➡️</a>
</p>

> **Tabular data, tamed.** Pandas is built on NumPy and gives you labeled, Excel-like tables in Python. This track takes you from your first `DataFrame` to cleaning messy real-world data, grouping, joining, time series, and file I/O.

---

## 🎯 What this track covers

Five sessions built around realistic, inline-generated datasets: no downloads required. You'll learn the `loc`/`iloc` split, the missing-data toolbox, split–apply–combine with `groupby`, `merge` for joining tables, and `resample`/`rolling` for time series.

---

## 📚 Sessions

| # | Session | Core topics | Mini project |
|:-:|---|---|---|
| **01** | [Series & DataFrames](./session-01-series-and-dataframes/README.md) | `Series`, `DataFrame`, creation from dict/CSV, `head`/`info`/`describe`, column selection | 🎓 Student Gradebook |
| **02** | [Indexing, Selecting & Filtering](./session-02-indexing-selecting-filtering/README.md) | `loc` vs `iloc`, Boolean masks, `isin`/`between`/`query`, sorting | 🛒 Sales Explorer |
| **03** | [Cleaning Data](./session-03-cleaning-data/README.md) | Missing values, dtype conversion, duplicates, `.str`, renaming | 🧹 Messy CSV Cleanup |
| **04** | [GroupBy & Merging](./session-04-groupby-merging/README.md) | `groupby`/`agg`/`transform`, `pivot_table`, `merge`/`join`, `concat` | 🧾 Orders + Customers |
| **05** | [Time Series & I/O](./session-05-time-series-and-io/README.md) | `to_datetime`, `.dt`, `resample`, `rolling`, CSV/Excel/Parquet | 📅 Daily Sales Time Series |

---

## 🧭 How to use this track

Each session has the same five files:

- `README.md` — theory (intuition → formal → code)
- `notebook.ipynb` — runnable **Build It** + **Experiment** cells
- `exercises.md` — 5 graded exercises + a mini project
- `exercises_solutions.ipynb` — full worked solutions
- `ai-learning.md` — prompts to learn the session with an AI assistant

> **Prerequisite:** finish the [NumPy Track](../01-numpy/README.md) first — pandas is built directly on NumPy arrays, and the indexing/broadcasting ideas carry over.

---

## ⚡ Quick reference

| Task | Idiomatic call |
|---|---|
| Create a DataFrame | `pd.DataFrame({"a": [1, 2], "b": [3, 4]})` |
| Inspect | `df.head()`, `df.info()`, `df.describe()` |
| Select by label / position | `df.loc[0, "a"]` / `df.iloc[0, 0]` |
| Filter | `df[df["a"] > 1]`, `df.query("a > 1")` |
| Group & aggregate | `df.groupby("k")["v"].agg(["mean", "sum"])` |
| Join tables | `a.merge(b, on="id", how="left")` |
| Time series | `df.resample("M").sum()`, `df["v"].rolling(7).mean()` |
| Save | `df.to_csv("out.csv", index=False)` |

> Prefer a single page? See the [Pandas Cheat Sheet](./cheatsheet.md).

---

<p align="center">
  <a href="./cheatsheet.md">📄 Pandas Cheat Sheet</a> ·
  <a href="../01-numpy/README.md">⬅️ NumPy Track</a> ·
  <a href="../README.md">🏠 Lab Home</a> ·
  <a href="../03-matplotlib/README.md">📊 Matplotlib Track ➡️</a>
</p>
