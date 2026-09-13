# 🧪 Exercises — Session 4: GroupBy & Merging

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy, coding)** Build a `sales` DataFrame with `region`, `product`, and `revenue` columns (at least 6 rows), then compute total revenue per region with **named aggregation**: `.agg(total=("revenue", "sum"))`.
2. **(Easy)** Using the same DataFrame, produce a count of each `product` within each `region` with `pd.crosstab(..., margins=True)`.
3. **(Medium, coding)** Compute four statistics of `revenue` per region — sum, mean, max, and count — in a **single** `.agg()` call. Then return only the regions whose total revenue is above the overall average region total.
4. **(Medium, coding)** Use `.transform("sum")` to add a `region_total` column back onto every row, then compute each row's `share_of_region` as `revenue / region_total`.
5. **(Advanced, conceptual)** In your own words, explain the difference between `.agg()` and `.transform()` on a `groupby` object. Then describe what happens to the number of rows when you merge two DataFrames with `how="inner"` versus `how="outer"`, and why an unmatched key behaves differently in each case.

## 🚀 Mini Project — Orders + Customers

**Objective:** Combine two related tables — an `orders` table and a `customers` table — into one analysis table, then summarize sales by region. This mirrors the everyday "join then aggregate" workflow of real data analysis.

**Steps:**

1. Build an `orders` DataFrame with `order_id`, `customer_id`, `product`, `quantity`, and `unit_price`. Add a `revenue` column equal to `quantity * unit_price`.
2. Build a `customers` DataFrame with `customer_id`, `name`, and `region`. Include at least one customer who has **no orders**.
3. Merge the two tables on `customer_id` using `how="left"` so that every order is kept, and inspect the result.
4. Use named aggregation to summarize each region: `total_revenue=("revenue", "sum")`, `orders=("order_id", "count")`, and `avg_order=("revenue", "mean")`.
5. Use `pivot_table` to show revenue by `region` (rows) and `product` (columns), with `aggfunc="sum"`, `fill_value=0`, and `margins=True`.
6. Bonus: merge again with `how="outer"` and explain which extra row(s) appear compared to the `how="left"` result, and why those rows contain `NaN`.
