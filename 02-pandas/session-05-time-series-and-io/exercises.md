# 🧪 Exercises — Session 5: Time Series & I/O

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy, coding)** Convert the list `["2024-01-01", "2024-02-15", "2024-03-20"]` into a datetime `Series` with `pd.to_datetime` (hint: pass a `pd.Series` so `.dt` is available). Print its `dtype` and then extract the year of each date with `.dt.year`.
2. **(Easy)** From a daily date range, use the `.dt` accessor to count how many entries fall on each weekday name.
3. **(Medium, coding)** Build a 30-day daily `sales` series with a `DatetimeIndex`, then compute the **weekly total** with `resample("W").sum()` and the **monthly mean** with `resample("ME").mean()`.
4. **(Medium, coding)** For that same series, add a 7-day rolling mean and a day-over-day percentage change. Print the first 10 rows and explain why the first few rolling values are `NaN`.
5. **(Advanced, conceptual)** You need to persist a large DataFrame of daily sales for a downstream analytics pipeline, and also email a small preview to a colleague who only uses Excel. Explain which file format you would choose for each case — CSV, Excel, or Parquet — and justify the choice in terms of human-readability, dtype preservation, and performance.

## 🚀 Mini Project — Daily Sales Time Series

**Objective:** Build a year-long daily sales series, analyze it with resampling and rolling statistics, and save it in multiple formats — a realistic "analyze then persist" workflow.

**Steps:**

1. Generate a `DatetimeIndex` for a full year (365 days) starting `2024-01-01`, and create a `sales` column with a mild upward trend plus weekly seasonality and random noise (use a fixed seed for reproducibility).
2. Set the date as the index and sort it, then compute the monthly total sales with `resample("ME").sum()`.
3. Add a 7-day rolling mean column to smooth the daily noise, and a day-over-day `pct_change` column.
4. Compute the best and worst sales day (using the original daily values) and print them with their dates.
5. Save the enriched DataFrame to **CSV**, **Excel**, and **Parquet** files, reload each one, and confirm the reloaded row counts match — then delete the temporary files.
6. Bonus: reload the CSV **without** `parse_dates` and print the `dtype` of the date column to show that dates come back as strings unless you ask for parsing.
