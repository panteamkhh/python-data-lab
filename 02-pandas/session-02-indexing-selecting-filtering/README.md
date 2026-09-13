# 📘 Session 2 — Indexing, Selecting & Filtering

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">🐼 Pandas Track</a> ·
  <a href="../session-01-series-and-dataframes/README.md">⬅️ Session 1</a> ·
  <a href="../session-03-cleaning-data/README.md">Session 3 ➡️</a>
</p>

> **Goal:** pull exactly the rows and columns you want using `loc`/`iloc`, boolean masks, `isin`, `between`, and `query`, then write results back safely and sort them.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Select rows and columns by **label** with `.loc` and by **position** with `.iloc`.
- Slice rows and columns, and explain why `.loc` slices include the endpoint while `.iloc` slices do not.
- Filter rows with boolean masks and combine conditions using `&`, `|`, and `~`.
- Use `isin`, `between`, and `query` for readable, expressive filtering.
- Assign values into a subset of rows with `.loc` under Copy-on-Write.
- Sort a DataFrame with `sort_values` by one or more columns.

---

## 🧠 Concept Explanation

### 2.1 The two indexers: `.loc` and `.iloc`

**Intuition first.** A DataFrame is a grid. You can point at a cell in two ways: by its **name** ("the row labeled `r3`, column `price`") or by its **position** ("the 4th row, 2nd column"). pandas gives each style its own accessor so there is never any ambiguity.

- **`.loc[row, col]`** works with **labels** (the values in `.index` and `.columns`).
- **`.iloc[row, col]`** works with **integer positions** (always counting from `0`).

```python
df.loc["r3", "price"]    # by label
df.iloc[2, 1]            # by position
```

**The row/column rule.** Inside either accessor, the part **before the comma selects rows** and the part **after the comma selects columns**. A bare `df.loc["r3"]` selects the whole row; a bare `df.loc[:, "price"]` selects the whole column.

### 2.2 Slicing — and one famous gotcha

Both accessors accept slices:

```python
df.loc["r1":"r3"]        # label slice — BOTH endpoints included
df.iloc[0:3]             # position slice — end EXCLUSIVE, like Python
```

The asymmetry is a classic beginner trap:

- `.loc` slices are **inclusive on both ends** because they are label-based (there is no "next label" beyond the last one to stop at).
- `.iloc` slices follow normal Python slicing and **exclude** the stop position.

Use `df.iloc[:, 0:2]` to grab the first two columns positionally, and `df.loc[:, ["a", "b"]]` to grab them by name.

### 2.3 Boolean filtering

A **boolean mask** is a `Series` of `True`/`False` with the same index as the DataFrame. Passing it inside `df[...]` keeps only the rows where the mask is `True`.

```python
df[df["price"] > 10]                 # keep expensive rows
```

Combine conditions with element-wise logical operators — **not** Python's `and`/`or`:

| Intent | Operator |
|---|---|
| AND | `&` |
| OR | `|` |
| NOT | `~` |

**Always wrap each condition in parentheses**, because `&` binds more tightly than `>`:

```python
df[(df["price"] > 10) & (df["city"] == "Berlin")]
df[(df["units"] > 100) | (df["price"] < 5)]
df[~(df["city"] == "Berlin")]
```

### 2.4 `isin`, `between`, and `query`

Three expressive alternatives to hand-written masks:

- **`isin([...])`** — keep rows whose value is in a given set. `df[df["city"].isin(["Berlin", "Paris"])]`
- **`between(a, b)`** — shorthand for `(x >= a) & (x <= b)`, inclusive by default. `df[df["price"].between(5, 20)]`
- **`query("...")`** — filter with a readable string expression that can reference column names directly, so you drop the repeated `df[...]`. `df.query("price > 10 and city == 'Berlin'")`

```python
df[df["city"].isin(["Berlin", "Paris"])]
df[df["price"].between(5, 20)]
df.query("units > 100 or price < 5")
```

> **Tip:** in `query`, use `and`/`or`/`not` and backticks for column names with spaces.

### 2.5 Assigning with `.loc`

To change a subset of values, combine `.loc` with a boolean mask. This is **the** Copy-on-Write-safe way to write into a DataFrame:

```python
df.loc[df["units"] > 100, "status"] = "high"     # only matching rows
df.loc[:, "price"] = df["price"] * 1.1           # a whole column
```

Avoid chained assignment (`df["price"][df["price"] > 100] = 0`) — under pandas 3.0 it writes to a temporary copy and the parent stays unchanged.

### 2.6 Sorting with `sort_values`

```python
df.sort_values("price")                          # ascending by default
df.sort_values("price", ascending=False)         # descending
df.sort_values(["city", "units"])                # multi-key: city, then units
```

`sort_values` returns a **new** DataFrame (it does not touch the original) unless you assign the result. Use `na_position="first"` to control where missing values land.

---

## ⚙️ Why This Matters

- **Filtering is 80% of analysis.** "Orders over $100 in Berlin" is a boolean mask; being fluent here is the core data-analysis skill.
- **`.loc` and `.iloc` are everywhere.** Nearly every pandas recipe — even in the official docs — uses them, so understanding the label/position split unlocks all of it.
- **CoW-safe assignment prevents silent bugs.** A filter-then-update pipeline that relies on chained assignment will quietly do nothing in pandas 3.x.
- **Readable filters reduce mistakes.** `between`, `isin`, and `query` express intent with less punctuation and fewer precedence traps than raw `&`/`|`.

---

## 🔍 Key Insights

- `.loc` = labels, `.iloc` = positions. Before the comma is rows, after is columns.
- `.loc` slices are **inclusive**; `.iloc` slices are **exclusive**.
- Use `&` / `|` / `~` (not `and` / `or` / `not`) and **parenthesize** each condition.
- Boolean masks align by index, which is why they always match the parent frame.
- Write into a DataFrame with `df.loc[mask, "col"] = value` — never chained assignment.
- `sort_values` does not modify in place; assign the result if you want the sorted order to stick.

---

## 📦 Summary

pandas offers two indexers for one grid: `.loc` (label-based, inclusive slices) and `.iloc` (position-based, exclusive slices), with rows before the comma and columns after. Rows are filtered with boolean masks built from comparisons and combined with `&`, `|`, and `~`, or expressed more readably with `isin`, `between`, and `query`. Subsets are updated safely by assigning through `df.loc[mask, "col"]`, which respects Copy-on-Write. Finally, `sort_values` orders a DataFrame by one or several columns without mutating the original.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Cell by label | `df.loc["r2", "price"]` |
| Cell by position | `df.iloc[1, 0]` |
| Row by label | `df.loc["r2"]` |
| Whole column | `df.loc[:, "price"]` or `df["price"]` |
| Label slice (inclusive) | `df.loc["r1":"r3"]` |
| Position slice (exclusive) | `df.iloc[0:3]` |
| Boolean mask | `df[df["price"] > 10]` |
| Combine conditions | `df[(df["a"] > 1) & (df["b"] == "x")]` |
| Membership | `df[df["city"].isin(["Berlin", "Paris"])]` |
| Range test | `df[df["price"].between(5, 20)]` |
| String filter | `df.query("units > 100 or price < 5")` |
| Conditional assignment | `df.loc[df["units"] > 100, "status"] = "high"` |
| Sort | `df.sort_values(["city", "units"], ascending=[True, False])` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-01-series-and-dataframes/README.md">⬅️ Session 1</a> ·
  <a href="../session-03-cleaning-data/README.md">Session 3 ➡️</a>
</p>
