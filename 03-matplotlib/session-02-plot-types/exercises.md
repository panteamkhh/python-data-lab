# 🧪 Exercises — Session 2: Plot Types & Choosing the Right Chart

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Generate 120 random points with `np.random.default_rng(0)` and draw a scatter plot of `y = 3x + noise`. Color the markers, set `alpha`, add a title and both axis labels, and turn on the grid.
2. **(Easy, coding)** Given `categories = ["Mon", "Tue", "Wed", "Thu", "Fri"]` and `visitors = [120, 98, 145, 130, 160]`, draw both a vertical bar chart and a horizontal bar chart. Label each axis appropriately.
3. **(Medium, coding)** Simulate 2,000 samples from two normal distributions with different means and standard deviations. Draw a histogram of the first with `density=True`, and a box plot comparing both groups on the same axes or in two subplots.
4. **(Medium, conceptual)** For each scenario, name the single best chart type from this session and justify it in one sentence: (a) the share of a budget spent on four departments; (b) the relationship between study hours and exam score; (c) the spread of delivery times for three couriers; (d) a temperature reading with a ±0.5 °C instrument error.
5. **(Advanced, coding)** Create a time series `y = np.sin(x)` with a growing uncertainty band `err = 0.05 * (x + 1)` for `x` in `[0, 2π]`. Plot the mean line, add an `errorbar` with `capsize=4`, and shade the band with `fill_between`. Add a legend explaining both encodings.

## 🚀 Mini Project — Distribution & Comparison

**Objective:** Build a single figure that both shows the *distribution* of a measured quantity and compares *categories* of that quantity — the two most common exploratory plots combined.

**Steps:**

1. Import `matplotlib.pyplot as plt` and `numpy as np`, and create a reproducible generator with `rng = np.random.default_rng(42)`.
2. Generate three groups of data, e.g. test scores for `"Class A"`, `"Class B"`, and `"Class C"`, each with ~250 samples and different means/spreads.
3. Create a figure with two axes side by side using `fig, axes = plt.subplots(1, 2, figsize=(11, 4.5))`.
4. **Left axes (distribution):** draw a histogram of *one* group with `bins=25` and `density=True`, and overlay a smooth reference curve (e.g. a Gaussian computed from the group's `mean()` and `std()`) using `ax.plot`.
5. **Right axes (comparison):** draw a box plot of all three groups with `tick_labels=["Class A", "Class B", "Class C"]`, then overlay the group means as markers.
6. Give each axes a clear title, axis labels, and a light grid.
7. Use `fig.suptitle(...)` to add one overall title, and `fig.tight_layout()` so nothing overlaps.
8. Save the figure to `distribution_comparison.png` with `dpi=150` and `bbox_inches="tight"`, then call `plt.show()`.
