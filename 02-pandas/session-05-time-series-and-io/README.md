# 📘 Session 5 — Time Series & I/O

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">🐼 Pandas Track</a> ·
  <a href="../session-04-groupby-merging/README.md">⬅️ Session 4</a>
</p>

> **Goal:** parse and index dates, use the `.dt` accessor, reshape time series with `resample` and `rolling`, measure change with `shift`/`diff`/`pct_change`, and persist DataFrames to CSV, Excel, and Parquet.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Convert text columns into real timestamps with `pd.to_datetime`.
- Extract components (year, month, weekday, quarter) with the `.dt` accessor.
- Set a `DatetimeIndex` and slice time ranges with date strings.
- Downsample or upsample a time series with `resample`.
- Smooth noisy data and compute moving statistics with `rolling`.
- Measure lag, change, and growth with `shift`, `diff`, and `pct_change`.
- Save and reload DataFrames with `to_csv`/`read_csv`, `to_excel`/`read_excel`, and `to_parquet`/`read_parquet`.

---

## 🧠 Concept Explanation

### 5.1 Parsing dates with `pd.to_datetime`

**Intuition first.** To pandas, the text `"2024-01-01"` is just a string — it has no idea that January comes before February. Converting that text into a real timestamp is the first step of every time-series task, because only then can pandas sort, subtract, and resample dates correctly.

**Formal explanation.** `pd.to_datetime` parses strings (and numbers) into the `datetime64[ns]` dtype.

```python
dates = pd.to_datetime(["2024-01-01", "2024-02-15", "2024-03-20"])
dates.dtype   # datetime64[ns]
```

Useful arguments:

- `errors="coerce"` turns unparseable values into `NaT` ("not a time") instead of raising.
- `format="%Y-%m-%d"` parses faster and avoids ambiguous day/month guessing.
- `pd.to_datetime(df["date"])` converts a whole column in one call.

`NaT` is pandas' missing-value marker for datetimes — the datetime equivalent of `NaN`.

### 5.2 The `.dt` accessor

Once a `Series` holds datetimes, the `.dt` accessor exposes calendar components and boolean flags:

```python
df["date"].dt.year          # 2024
df["date"].dt.month         # 1, 2, 3, ...
df["date"].dt.day_name()    # "Monday", "Tuesday", ...
df["date"].dt.dayofweek     # 0 = Monday ... 6 = Sunday
df["date"].dt.quarter       # 1, 2, 3, 4
df["date"].dt.is_month_end  # True / False
df["date"].dt.floor("D")    # drop the time part
```

`.dt` only works on datetime-like columns. Calling it on a string column raises an error — a reminder to run `pd.to_datetime` first.

### 5.3 Setting a `DatetimeIndex`

Time-series methods such as `resample` require a `DatetimeIndex`. Turn a date column into the index with `set_index`:

```python
ts = df.set_index("date").sort_index()
```

Sorting the index matters: many time operations assume chronological order. A `DatetimeIndex` also unlocks convenient **partial-string slicing**:

```python
ts.loc["2024-01-05":"2024-01-08"]   # a date range
ts.loc["2024-02"]                    # everything in February
```

You can also build one from scratch with `pd.date_range("2024-01-01", periods=365, freq="D")`.

### 5.4 `resample` — groupby for time

`resample` is to timestamps what `groupby` is to categories: it defines time buckets and aggregates within each bucket.

```python
ts["sales"].resample("W").sum()          # weekly totals
ts["sales"].resample("ME").mean()        # monthly averages
ts["sales"].resample("MS").sum()         # monthly, labeled at month start
ts["sales"].resample("W").agg(["sum", "mean", "max"])
```

Common frequency aliases: `"D"` (day), `"W"` (week ending Sunday), `"ME"` (month end), `"MS"` (month start), `"QE"` (quarter end), `"YE"` (year end). For aggregated series, `upsampling` (going from monthly to daily) introduces `NaN`s unless you fill them, e.g. with `.ffill()`.

### 5.5 `rolling` — moving windows

Where `resample` creates **disjoint** buckets, `rolling` slides a **fixed-size window** along the series and aggregates each window, producing a value for every original row.

```python
ts["sales"].rolling(window=7).mean()                    # 7-row moving average
ts["sales"].rolling(window=7, min_periods=1).mean()     # no leading NaNs
ts["sales"].rolling("7D").mean()                        # 7-calendar-day window
```

The first `window - 1` values are `NaN` by default because a full window is not yet available; `min_periods=1` allows partial windows to produce a value. Rolling means are the standard way to smooth a noisy daily signal.

### 5.6 `shift`, `diff`, and `pct_change`

These three methods express a series **relative to its own past**:

| Method | Meaning | Formula |
|---|---|---|
| `.shift(1)` | lag by one period | value at row `i-1` |
| `.diff()` | absolute change | `x[i] - x[i-1]` |
| `.pct_change()` | relative change | `(x[i] - x[i-1]) / x[i-1]` |

```python
ts["sales"].shift(1)        # yesterday's value, aligned to today
ts["sales"].diff()          # day-over-day change
ts["sales"].pct_change()    # day-over-day growth rate
```

`shift` powers lag features in forecasting; `diff` removes trend; `pct_change` gives growth rates. `pct_change()` divides by the shifted value, so divide-by-zero (and a preceding `NaN`) yields `NaN` or `inf`.

### 5.7 CSV — `to_csv` / `read_csv`

**CSV** is plain text: human-readable, universally compatible, but untyped.

```python
ts.reset_index().to_csv("sales.csv", index=False)
back = pd.read_csv("sales.csv", parse_dates=["date"])
```

- `index=False` avoids writing the row index as an unnamed first column.
- `parse_dates=["date"]` converts the date column back from text to `datetime64` on load.
- Everything else is inferred — dtypes are **not** guaranteed to round-trip. Use `dtype=` on read if you need control.

### 5.8 Excel — `to_excel` / `read_excel`

**Excel** files (`.xlsx`) are ideal for sharing with non-programmers. pandas uses the `openpyxl` engine.

```python
ts.reset_index().to_excel("sales.xlsx", index=False, sheet_name="Sales")
back = pd.read_excel("sales.xlsx", sheet_name="Sales", parse_dates=["date"])
```

A workbook can hold multiple sheets, selected by name or index with `sheet_name=`. Excel stores numbers as floating point and does **not** support timezone-aware timestamps or very large row counts (about one million rows per sheet).

### 5.9 Parquet — `to_parquet` / `read_parquet`

**Parquet** is a columnar, compressed, typed binary format — the default choice for serious data work. pandas uses the `pyarrow` engine.

```python
ts.reset_index().to_parquet("sales.parquet", index=False)
back = pd.read_parquet("sales.parquet")
```

Parquet preserves dtypes exactly, stores only the columns you read (`columns=[...]`), and is far faster and smaller than CSV or Excel for large tables. The trade-off is that it is not human-readable.

| Format | Human-readable? | Preserves dtypes? | Best for |
|---|---|---|---|
| **CSV** | Yes | No (inferred on read) | Sharing, small files, interoperability |
| **Excel** | Yes | Mostly (dates need parsing) | Business reporting, non-technical users |
| **Parquet** | No | Yes | Large data, pipelines, analytics engines |

---

## ⚙️ Why This Matters

- **Business reporting:** weekly and monthly roll-ups from daily transactions are a single `resample(...).sum()` call.
- **Forecasting & analytics:** `shift` and `diff` create the lag and change features that forecasting models depend on.
- **Signal smoothing:** `rolling` turns a noisy sensor or sales feed into a readable trend line.
- **Data pipelines:** Parquet is the interchange format of modern data platforms (Spark, DuckDB, cloud warehouses), while CSV/Excel remain the formats you exchange with humans.
- **Reproducible analysis:** choosing the right file format and parsing dates correctly at load time prevents silent, hard-to-find timezone and dtype bugs.

---

## 🔍 Key Insights

- Dates stored as **strings** sort incorrectly (`"10"` before `"2"` in some formats) and cannot be resampled — always run `pd.to_datetime` first.
- `.dt` is available only on datetime-typed `Series`; on a string column it raises rather than guessing.
- `resample` needs a **`DatetimeIndex`**; it is a `groupby` over time buckets, not an index-based slice.
- `resample` uses **disjoint** buckets, while `rolling` uses **overlapping** windows — the same `"W"`/`7` cadence produces very different results.
- `rolling` produces leading `NaN`s until a full window exists; set `min_periods=1` when you want every row populated.
- `diff()` is literally `x - x.shift(1)`; `pct_change()` is `diff() / shift(1)`. Understanding one explains the others.
- CSV is untyped text — **dtypes do not round-trip**; Parquet does preserve them. Excel sits in between and cannot store timezone-aware datetimes.
- `parse_dates=` on `read_csv`/`read_excel` is the reload-time counterpart to `to_datetime` — forgetting it leaves your dates as strings.

---

## 📦 Summary

Time-series work in pandas starts with conversion: `pd.to_datetime` turns text into `datetime64`, the `.dt` accessor extracts calendar components, and `set_index("date")` installs a `DatetimeIndex` that enables date slicing. With that index in place, `resample` aggregates into fixed time buckets (daily, weekly, monthly), `rolling` computes moving statistics over sliding windows, and `shift`/`diff`/`pct_change` describe a series relative to its own past. For persistence, `to_csv`/`read_csv` and `to_excel`/`read_excel` produce human-readable files at the cost of inferred dtypes, while `to_parquet`/`read_parquet` delivers a typed, compressed, high-performance binary format. Choosing the right format — and parsing dates correctly on read — completes a robust analysis workflow.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Parse text dates | `pd.to_datetime(df["date"], errors="coerce")` |
| Calendar parts | `df["date"].dt.year`, `.dt.month`, `.dt.day_name()` |
| Build a date index | `ts = df.set_index("date").sort_index()` |
| Slice a date range | `ts.loc["2024-01-01":"2024-02-01"]` |
| Resample | `ts["sales"].resample("W").sum()` |
| Rolling average | `ts["sales"].rolling(7).mean()` |
| Lag / change / growth | `.shift(1)`, `.diff()`, `.pct_change()` |
| Save / load CSV | `df.to_csv("f.csv", index=False)` / `pd.read_csv("f.csv", parse_dates=["date"])` |
| Save / load Excel | `df.to_excel("f.xlsx", index=False)` / `pd.read_excel("f.xlsx", parse_dates=["date"])` |
| Save / load Parquet | `df.to_parquet("f.parquet", index=False)` / `pd.read_parquet("f.parquet")` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-04-groupby-merging/README.md">⬅️ Session 4</a> ·
  <a href="../../README.md">🏠 Back to Home</a>
</p>
