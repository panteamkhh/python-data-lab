# 🧪 Exercises — Session 2: Indexing, Selecting & Filtering

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

Use this starter DataFrame for every exercise:

```python
import pandas as pd

df = pd.DataFrame(
    {
        "city": ["Berlin", "Paris", "Berlin", "Lisbon", "Paris", "Berlin"],
        "price": [12.5, 30.0, 7.5, 22.0, 15.0, 40.0],
        "units": [120, 45, 300, 80, 200, 15],
    },
    index=["o1", "o2", "o3", "o4", "o5", "o6"],
)
```

1. **(Easy)** Select the `price` of order `"o3"` using `.loc`, then select the same value using `.iloc`. Confirm they are equal.
2. **(Easy)** Select the rows `"o2"` through `"o4"` with a `.loc` slice, and the first three rows with an `.iloc` slice. Explain why the endpoints behave differently.
3. **(Medium, coding)** Filter the DataFrame to orders with `price` above `15` **and** `units` below `100`. Then find orders that are in `["Berlin", "Lisbon"]` **or** have `price` under `10`.
4. **(Medium)** Use `between` to keep orders priced between `10` and `25` inclusive, then rewrite the same filter as a `query("...")` string. Sort the result by `units` descending.
5. **(Advanced, conceptual)** Explain the difference between `df.loc[mask, "status"] = "high"` and `df["status"][mask] = "high"` under Copy-on-Write, and say which one actually updates the DataFrame.

## 🚀 Mini Project — Sales Explorer

**Objective:** Build a small sales table, then answer real business questions with `loc`/`iloc`, boolean filters, `query`, conditional assignment, and sorting.

**Steps:**

1. Create a `DataFrame` from a dict of lists with columns `order_id`, `city`, `product`, `price`, and `units` (invent 8 rows across at least 3 cities).
2. Use `.loc` to select and print all columns for the order with a specific `order_id` (use the first order's ID).
3. Use `.iloc` to print the last 3 rows and the first 4 rows, only the `city` and `price` columns.
4. Filter to rows where `price > 20` and `units > 10`; call this `big_orders`.
5. Use `.isin` to find orders in two chosen cities, and `between` to find orders priced `10`–`30`.
6. Add a `"tier"` column with `.loc`: `"premium"` where `price >= 25`, else `"standard"`.
7. Add a `"revenue"` column as `price * units`, then sort by `revenue` descending and print the top 3.
8. Print the total revenue per city (hint: `df.groupby("city")["revenue"].sum()` — previewed here, covered fully in Session 4).
