# 🧪 Exercises — Session 4: Styling & Colormaps

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Plot the line `y = x**2` for `x = np.linspace(0, 5, 50)` twice on the same axes: once with a named color and a dashed line, once with a hex color and a dotted line. Add a legend so the two lines are distinguishable.
2. **(Easy)** Print `len(plt.style.available)` and the first five theme names. Then reproduce one simple line chart under two different style sheets using two separate `with plt.style.context(...)` blocks.
3. **(Medium, coding)** Generate 1000 random points from a 2D normal distribution with `np.random.default_rng(...)`. Draw a scatter plot colored by a third variable using `cmap="viridis"`, set `alpha=0.6`, and attach a labeled colorbar.
4. **(Medium)** Create a 2D array `Z = np.random.default_rng(7).random((12, 12))`. Display it with `imshow(..., origin="lower", cmap="magma")`, add a colorbar, and set the title to describe the array's shape.
5. **(Advanced, conceptual)** Explain, in your own words, when you would reach for `imshow` and when you would reach for `pcolormesh`, including what each function assumes about the x/y coordinates of the data. Give one concrete example of a dataset that suits each.

## 🚀 Mini Project — Chart Makeover

**Objective:** Take a deliberately plain, ugly chart and transform it into a clear, polished, presentation-ready figure using everything from this session — colors, alpha, line styles, markers, `rcParams`, a style sheet, and a saved high-resolution output.

**Steps:**

1. Build the "before" chart: a single line plot of `y = np.sin(x) * x` over `x = np.linspace(0, 10, 200)` with the Matplotlib defaults and no styling at all.
2. Rebuild the exact same data as the "after" chart, but this time:
   - apply a style sheet with `with plt.style.context(...)`;
   - recolor the line (named or hex), set `linewidth=2.5`, and add circle markers on every 20th point using `markevery`;
   - add a light grid and remove the top and right spines;
   - give the figure a bold title and axis labels.
3. Add a shaded region behind the curve using `ax.fill_between(x, y, alpha=0.15)` to show how `alpha` adds depth.
4. Display the "before" and "after" charts side by side in a `1 × 2` subplot grid so the makeover is obvious.
5. Save the final figure to the current working directory with `fig.savefig("chart_makeover.png", dpi=150, bbox_inches="tight")`, then display it, then delete the file with `os.remove("chart_makeover.png")` so no stray files are left behind.
