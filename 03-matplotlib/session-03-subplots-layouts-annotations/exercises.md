# 🧪 Exercises — Session 3: Subplots, Layouts & Annotations

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Create a `2 × 2` grid of subplots with `figsize=(10, 6)`. In each panel, plot a different simple function (`x`, `x²`, `x³`, and `sin(x)`) over `[0, 1]`.
2. **(Easy, coding)** Create a `1 × 2` grid with `sharey=True`, plotting two different noisy signals over the same x-range. Add a single shared y-label with `fig.supylabel(...)`.
3. **(Medium, coding)** Use `GridSpec` to build a layout where the top panel spans the full width and the bottom row has two equal panels. Plot a wide time series on top and its histogram and its autocorrelation-style delay plot below.
4. **(Medium, conceptual)** Explain what `sharex=True` changes and why it is considered good practice when comparing two panels. Describe one situation where sharing an axis would be *misleading*.
5. **(Advanced, coding)** Plot `y = np.sin(x)` over `[0, 2π]` and annotate the maximum and minimum with `ax.annotate()` and arrows. Add a dashed vertical line at the maximum and a shaded `axvspan` highlighting the region where `sin(x) > 0.5`.

## 🚀 Mini Project — 2×2 Dashboard

**Objective:** Assemble a single, polished 2×2 dashboard that combines several chart types, shared axes, a twin axis, and annotations into one self-explanatory figure.

**Steps:**

1. Import `matplotlib.pyplot as plt` and `numpy as np`, and create `rng = np.random.default_rng(3)`.
2. Build the figure with `fig, axes = plt.subplots(2, 2, figsize=(12, 8), layout="constrained")`.
3. **Top-left — line plot:** simulate a time series (e.g. a trend plus noise) and plot it with a horizontal reference line at the mean using `ax.axhline`.
4. **Top-right — scatter:** plot two correlated variables and mark one highlighted outlier with `ax.annotate(..., arrowprops=...)`.
5. **Bottom-left — histogram:** show the distribution of the time-series values with `bins=30` and `density=True`.
6. **Bottom-right — twin axis:** plot a cumulative series on the left y-axis and its rolling average on a `twinx()` right y-axis, coloring each label to match its line.
7. Give every panel a title and axis labels, and turn on a light grid in each.
8. Add one overall title with `fig.suptitle("Operational Dashboard", fontsize=14)`.
9. Save the figure to `dashboard.png` with `dpi=150` and `bbox_inches="tight"`, then call `plt.show()`.
