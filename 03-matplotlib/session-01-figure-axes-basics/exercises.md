# 🧪 Exercises — Session 1: Figure/Axes Model & First Plots

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Using the object-oriented API, create a figure with `plt.subplots()` and plot `y = x²` for `x` from `-5` to `5`. Add a title, both axis labels, and turn on the grid.
2. **(Easy, coding)** On a single `Axes`, plot both `sin(x)` and `cos(x)` for `x` in `[0, 2π]`. Give each line a different color and line style, add a `label=` to each, and draw a legend.
3. **(Medium, coding)** Plot `y = 2^x` for `x` from `0` to `10` and set the y-axis to a **logarithmic** scale. Then save the figure as `exponential.png` at `dpi=150` with `bbox_inches="tight"`.
4. **(Medium, conceptual)** Explain the difference between `plt.plot(x, y)` and `fig, ax = plt.subplots(); ax.plot(x, y)`. In your answer, name two concrete advantages of the object-oriented style.
5. **(Advanced, coding)** Write a reusable function `plot_function(func, xmin, xmax, title)` that builds `x = np.linspace(xmin, xmax, 400)`, creates a styled figure (title, labels, light grid), plots `func(x)`, sets the limits to `xmin`/`xmax` on the x-axis, and **returns** `(fig, ax)` instead of showing the plot. Test it with `np.sin` and `np.tanh`.

## 🚀 Mini Project — Plot a Function

**Objective:** Produce a polished, saved plot of a mathematical function using the object-oriented API — the kind of figure you would drop into a report. You will choose a function, style the line, label everything, and export a PNG.

**Steps:**

1. Import `matplotlib.pyplot as plt` and `numpy as np`.
2. Define the domain with `x = np.linspace(-2 * np.pi, 2 * np.pi, 500)`.
3. Choose a function, e.g. `y = np.sin(x) * np.exp(-x**2 / 20)` (a damped sine). Compute `y`.
4. Create the figure with `fig, ax = plt.subplots(figsize=(9, 4.5))`.
5. Plot `y` with a styled line: a distinctive `color`, `linewidth=2.5`, and `label="sin(x) · e^(-x²/20)"`.
6. Add a title, an x-axis label, and a y-axis label.
7. Add a light dashed grid and a legend in the `"upper right"` corner.
8. Use `ax.set_xlim(...)` and `ax.set_ylim(...)` to give the curve a little breathing room.
9. Mark the origin with horizontal and vertical reference lines if you like (`ax.axhline(0, ...)`, `ax.axvline(0, ...)`).
10. Save the result to `function_plot.png` with `dpi=150` and `bbox_inches="tight"`, then call `plt.show()`.
