# 📘 Session 3 — Subplots, Layouts & Annotations

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">📊 Matplotlib Track</a> ·
  <a href="../session-02-plot-types/README.md">⬅️ Session 2</a> ·
  <a href="../session-04-styling-and-colormaps/README.md">Session 4 ➡️</a>
</p>

> **Goal:** arrange multiple plots in grids and custom layouts, share axes, add a secondary y-axis, and annotate the story with text, arrows, and spans.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Create regular subplot grids with `plt.subplots(nrows, ncols)` and index the resulting axes array.
- Share axes between panels with `sharex=True` / `sharey=True` for honest comparisons.
- Control panel spacing with `constrained_layout=True`, `fig.tight_layout()`, and `subplots_adjust`.
- Build non-uniform layouts with `GridSpec` and `add_gridspec`.
- Overlay a second data scale with `ax.twinx()`.
- Point at data with `ax.annotate()`, `ax.text()`, and `arrowprops`.
- Mark regions and thresholds with `ax.axhline()`, `ax.axvline()`, and `ax.axvspan()`.

---

## 🧠 Concept Explanation

### 3.1 Subplot grids with `plt.subplots()`

**Intuition first.** Think of a figure as a page and subplots as framed pictures arranged in a grid. You decide the grid shape (`nrows × ncols`), and Matplotlib hands you the frames so you can draw in each one.

**Formal explanation.** `plt.subplots()` returns a `Figure` and an array of `Axes`:

```python
fig, axes = plt.subplots(nrows=2, ncols=3, figsize=(12, 6))

# axes is a 2D NumPy array of shape (2, 3)
ax = axes[0, 1]          # row 0, column 1
```

Indexing rules:

- `nrows == 1` or `ncols == 1` still returns a 2D array (shape `(1, n)` or `(n, 1)`) unless you call `squeeze`.
- The common idiom `fig, axs = plt.subplots(2, 2)` gives `axs[i, j]`.
- Loop over panels with `for ax in axes.ravel(): ax.grid(True)` to apply shared styling.

### 3.2 Sharing axes with `sharex` / `sharey`

When panels show the same variable, sharing the axis makes them directly comparable and removes duplicate tick labels:

```python
fig, axes = plt.subplots(1, 2, sharex=True, sharey=True, figsize=(10, 4))
```

- `sharex=True` links the x-limits: zooming or panning one panel affects all.
- Shared axes are a form of **honest comparison** — different limits can make the same difference look large or small.
- Use `fig.supxlabel()` / `fig.supylabel()` to label the whole grid instead of repeating labels.

### 3.3 Layout control: `constrained_layout` and `tight_layout`

By default, subplots can overlap their labels and titles. Two mechanisms fix this:

```python
fig, axes = plt.subplots(2, 2, layout="constrained")   # modern, recommended
```

or, after building the figure:

```python
fig.tight_layout()          # adjusts padding in one shot
```

- `layout="constrained"` (equivalently `constrained_layout=True`) is an automatic layout engine that reserves space for titles, labels, and colorbars from the start.
- `fig.tight_layout()` is a simpler one-pass adjustment; call it *before* `savefig`.
- Manual control is still available: `fig.subplots_adjust(hspace=0.4, wspace=0.3)`.

### 3.4 Custom layouts with `GridSpec`

`GridSpec` lets panels span multiple rows or columns — perfect for dashboards with one wide hero plot and several small ones:

```python
fig = plt.figure(figsize=(10, 6), layout="constrained")
gs = fig.add_gridspec(2, 3)

ax_top = fig.add_subplot(gs[0, :])       # spans all 3 columns of row 0
ax_left = fig.add_subplot(gs[1, 0])
ax_mid = fig.add_subplot(gs[1, 1])
ax_right = fig.add_subplot(gs[1, 2])
```

- `gs[0, :]` selects an entire row; `gs[:, 0]` an entire column.
- `fig.add_gridspec(..., height_ratios=[2, 1])` gives the top row twice the height.
- `GridSpec` also supports `width_ratios` and a `wspace`/`hspace` argument.

### 3.5 A second y-axis with `twinx()`

When two series have different units, a shared x-axis with two y-axes is clearer than normalizing the data:

```python
fig, ax1 = plt.subplots()
ax2 = ax1.twinx()          # shares the x-axis, independent y-axis

x = np.linspace(0, 10, 100)
ax1.plot(x, np.exp(-x / 5), color="tab:blue", label="decay")
ax1.set_ylabel("decay (a.u.)", color="tab:blue")

ax2.plot(x, x ** 2, color="tab:red", label="growth")
ax2.set_ylabel("growth", color="tab:red")

fig.tight_layout()
plt.show()
```

- `twinx()` returns a **new** axes that shares the x-axis.
- `twiny()` is the transpose: shares y, independent x.
- Merge both legends with `lines1 + lines2, labels1 + labels2` in a single `ax1.legend(...)`.

### 3.6 Annotations: `text`, `annotate`, and arrows

Data tells a story; annotations point at the important part.

```python
fig, ax = plt.subplots()
x = np.linspace(0, 2 * np.pi, 200)
ax.plot(x, np.sin(x))

# Free-floating text in data coordinates
ax.text(np.pi / 2, 1.05, "peak", ha="center", color="tab:green")

# Text with an arrow pointing at a specific data point
ax.annotate(
    "minimum",
    xy=(1.5 * np.pi, -1.0),          # point to annotate
    xytext=(1.5 * np.pi, -0.4),      # where the text sits
    ha="center",
    arrowprops=dict(arrowstyle="->", color="black"),
)
plt.show()
```

- `ax.text(x, y, s)` places text at data coordinates; use `transform=ax.transAxes` with `(0.5, 0.95)` for axes-relative coordinates.
- `ax.annotate` takes `xy` (the target) and `xytext` (the label position); `arrowprops` styles the connector.
- Useful `ha` (horizontal alignment) values: `"left"`, `"center"`, `"right"`.

### 3.7 Reference lines and shaded regions

`axhline`, `axvline`, `axvspan`, and `axhspan` mark thresholds and regimes without adding data series:

```python
fig, ax = plt.subplots()
x = np.linspace(0, 10, 200)
ax.plot(x, np.sin(x))

ax.axhline(0, color="gray", linewidth=0.8)          # horizontal line
ax.axvline(np.pi, color="tab:red", linestyle="--")  # vertical line
ax.axvspan(2, 4, color="tab:orange", alpha=0.2)     # shaded region
ax.axhspan(-0.5, 0.5, color="tab:green", alpha=0.15)
plt.show()
```

- `axhline(y)` draws across the full x-range; `axvline(x)` across the full y-range.
- `axvspan(xmin, xmax)` shades a vertical band; `axhspan(ymin, ymax)` shades a horizontal band.
- All three accept the usual `color`, `linestyle`, `linewidth`, and `alpha`.

---

## ⚙️ Why This Matters

- **Dashboards:** real reports rarely show one plot — they combine several panels that share context, which is exactly what grids and shared axes provide.
- **Honest comparison:** shared limits prevent the classic mistake of making one panel look "bigger" by changing its range.
- **Different units, one story:** `twinx` is how you plot temperature and humidity, or price and volume, on one figure.
- **Annotations turn charts into arguments:** a labeled peak or threshold tells the reader what to conclude, not just what the data is.
- **Publication layouts:** `GridSpec` and constrained layout are how you build a multi-panel figure that survives peer review.

---

## 🔍 Key Insights

- `plt.subplots(2, 2)` returns a 2D `axes` array; index it as `axes[row, col]`.
- Sharing an axis edits all panels at once — a feature when comparing, a trap when panels show different quantities.
- `layout="constrained"` is the modern default recommendation; it handles colorbars and long labels better than `tight_layout`.
- `GridSpec` subscripts select spans: `gs[0, :]` is the full first row.
- `twinx()` creates a *new* axes; it does not modify `ax1`. Remember to label each y-axis with a matching color.
- `ax.text` uses data coordinates by default; add `transform=ax.transAxes` for coordinates in the 0–1 range of the axes.
- `axvspan`/`axhspan` are drawn behind the data by default (`zorder`), so they make clean background highlights.

---

## 📦 Summary

Subplots arrange several `Axes` inside one `Figure`, created with `plt.subplots()` and indexed as a NumPy array. `sharex`/`sharey` link panels for honest comparison, while `constrained_layout` and `tight_layout` keep labels from overlapping. When a uniform grid is not enough, `GridSpec` builds custom layouts with row and column spans. `twinx()` adds an independent second y-axis, `annotate`/`text`/arrows explain the data, and `axhline`/`axvline`/`axvspan` mark thresholds and regions. Together these tools turn a collection of plots into a coherent, self-explanatory figure.

---

## ⚡ Quick Reference

| Task | Call |
|---|---|
| Create a grid | `fig, axes = plt.subplots(2, 2, figsize=(10, 6))` |
| Access a panel | `ax = axes[row, col]` |
| Share an axis | `plt.subplots(1, 2, sharex=True, sharey=True)` |
| Auto spacing | `plt.subplots(..., layout="constrained")` or `fig.tight_layout()` |
| Custom spans | `gs = fig.add_gridspec(2, 3); fig.add_subplot(gs[0, :])` |
| Second y-axis | `ax2 = ax1.twinx()` |
| Text | `ax.text(x, y, "label")` |
| Arrowed annotation | `ax.annotate("t", xy=(x, y), xytext=(x2, y2), arrowprops=dict(arrowstyle="->"))` |
| Reference lines | `ax.axhline(0)`, `ax.axvline(np.pi)` |
| Shaded region | `ax.axvspan(2, 4, alpha=0.2)`, `ax.axhspan(-1, 1, alpha=0.2)` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-04-styling-and-colormaps/README.md">Session 4 ➡️</a>
</p>
