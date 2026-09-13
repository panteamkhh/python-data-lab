# 🧪 Exercises — Session 5: Data Storytelling & Dashboards

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Build a small `DataFrame` inline with the columns `month` (1–6) and `revenue` (any six numbers). Plot `revenue` against `month` in two equivalent ways: once with `ax.plot(df["month"], df["revenue"])` and once with `df.plot(x="month", y="revenue", ax=ax)`. Confirm the two lines land on the same axes.
2. **(Easy)** Create a `2 × 1` subplot grid with `sharex=True`. Plot an upward trend in the top panel and a downward trend in the bottom panel, using the same x values. Explain in one sentence how `sharex=True` changed the tick labels.
3. **(Medium, coding)** Build a bar chart of total revenue per product from a small inline `DataFrame`. Sort the bars by value, start the y-axis at zero, add a value label on top of each bar, and give the chart a bold title and a gray subtitle.
4. **(Medium)** On a line chart of monthly totals, annotate the maximum point with `ax.annotate(...)` (an arrow plus the value), and add a dashed `axhline` at the mean value. Make sure both annotations are readable and do not overlap the line.
5. **(Advanced, conceptual)** A colleague shows you a bar chart whose y-axis starts at 90 instead of 0 to make a difference look dramatic. In your own words, explain why this is misleading, how you would redraw the chart honestly, and one situation where a non-zero baseline *is* acceptable.

## 🚀 Mini Project — Sales Dashboard

**Objective:** Build a multi-panel **sales dashboard** from a `DataFrame` generated inline with pandas and NumPy. The dashboard should combine at least four views of the same data, guide the reader with a title, per-panel titles, an annotation, and a source caption, and export a publication-quality PNG.

**Steps:**

1. Generate the data entirely inline: use `np.random.default_rng(...)` and `pd.date_range(...)` to build a tidy `DataFrame` with columns `month`, `product`, and `revenue` covering 12 months and 3 products.
2. Pivot or group the data as needed — for example, a wide table with one column per product, a total per product, and a monthly total.
3. Create a `2 × 2` figure with `plt.subplots(2, 2, figsize=(13, 8))` and fill the panels:
   - top-left: a line chart of monthly revenue per product (`pivot.plot(ax=ax, marker="o")`);
   - top-right: a bar chart of total revenue per product, sorted and starting at zero;
   - bottom-left: a bar chart of monthly totals with the peak annotated via `ax.annotate(...)`;
   - bottom-right: a heatmap of revenue by product and month using `imshow` or `pcolormesh`, with a `colorbar`.
4. Add a bold `fig.suptitle(...)` headline and a gray `fig.text(...)` source caption at the bottom, and use `fig.tight_layout(rect=[0, 0.03, 1, 0.94])` so nothing overlaps.
5. Save the dashboard with `fig.savefig("sales_dashboard.png", dpi=200, bbox_inches="tight")`, display it with `plt.show()`, then delete the file with `os.remove("sales_dashboard.png")` so no stray files remain.
