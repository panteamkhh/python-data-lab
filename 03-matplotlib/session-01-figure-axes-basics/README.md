# 📘 Session 1 — The Figure/Axes Model & First Plots

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">📊 Matplotlib Track</a> ·
  <a href="../session-02-plot-types/README.md">Session 2 ➡️</a>
</p>

> **Goal:** understand the Figure/Axes object model, build a plot with the object-oriented API, style its labels and axes, and save it to a PNG.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Explain what a Matplotlib `Figure` and `Axes` are, and how they relate to each other.
- Describe the difference between the **pyplot state machine** and the **object-oriented** API — and why the OO style is preferred.
- Create a figure and one or more axes with `plt.subplots()`.
- Draw lines with `ax.plot()` and control color, linewidth, linestyle, and marker.
- Add an axis label, title, legend, and grid to a plot.
- Control the visible range with `xlim`/`ylim` and switch an axis to a logarithmic scale.
- Save a figure with `fig.savefig()`, tuning `dpi` and `bbox_inches`.

---

## 🧠 Concept Explanation

### 1.1 What is Matplotlib?

**Intuition first.** Think of a chart as a *picture frame* containing a *canvas*, and on that canvas there is a *drawing area* with axes, ticks, and lines. Matplotlib gives you a handle on each of those pieces so you can change any of them independently.

**Formal explanation.** Matplotlib is Python's most widely used 2D plotting library. Almost everything in Matplotlib is an **Artist** — an object that knows how to draw itself onto the canvas. The two artists you will meet first are:

- **`Figure`** — the entire image, the top-level container. It holds everything you see, including one or more axes. Think of it as the *page*.
- **`Axes`** — a single plot with its own x-axis, y-axis, title, and data. Think of it as the *panel* on the page. Confusingly, one `Axes` object is *not* the same as the plural "axes" (the x/y lines) it contains.

A figure can contain many `Axes`. The layout objects are:

| Object | What it is | Analogy |
|---|---|---|
| `Figure` | The whole image / window | A sheet of paper |
| `Axes` | One plot panel (with x/y axes) | A framed picture on the paper |
| `Axis` | The x-axis or y-axis (ticks, labels, limits) | The ruler along one edge |
| `Artist` | Anything drawable (line, text, legend, ...) | The ink marks |

### 1.2 Two ways to drive Matplotlib

Matplotlib can be used through two interfaces:

1. **The pyplot state machine** — a set of functions like `plt.plot()`, `plt.title()`, `plt.xlabel()` that act on "the current figure/axes". Convenient for quick, throwaway plots.
2. **The object-oriented (OO) API** — you explicitly create a `Figure` and `Axes`, then call methods on those objects: `fig, ax = plt.subplots()` followed by `ax.plot(...)`, `ax.set_title(...)`.

Both interfaces coexist, but the **OO style is the recommended default** for anything you intend to keep, because:

- It is explicit: you always know *which* axes you are drawing on.
- It scales naturally to multiple subplots and complex layouts.
- It is far easier to refactor into functions and reuse.

We will use the OO style throughout this track, and only mention the state-machine equivalents when they are genuinely shorter.

```python
import matplotlib.pyplot as plt
import numpy as np

# Object-oriented style (preferred)
fig, ax = plt.subplots()
ax.plot([0, 1, 2], [0, 1, 4])
ax.set_title("My first OO plot")
plt.show()
```

### 1.3 Creating a figure with `plt.subplots()`

`plt.subplots()` creates a `Figure` and a grid of `Axes` in a single call and returns them as a tuple:

```python
fig, ax = plt.subplots()          # one figure, one axes
```

With arguments you can control the size and the grid:

```python
fig, ax = plt.subplots(figsize=(8, 4), dpi=100)      # 8x4 inches, 100 dots per inch
fig, axes = plt.subplots(nrows=2, ncols=2)           # a 2x2 grid -> axes is a 2D array
```

- `figsize` is `(width, height)` in **inches**.
- `dpi` is dots per inch; the pixel size is `figsize * dpi`.
- If there is exactly one axes, `plt.subplots()` returns a bare `Axes`; for a grid it returns a NumPy array of `Axes`.

### 1.4 Drawing with `ax.plot()`

`ax.plot(x, y)` draws a line connecting the points in `y` at positions `x`. Many visual properties can be set in one call:

```python
x = np.linspace(0, 2 * np.pi, 200)

fig, ax = plt.subplots()
ax.plot(x, np.sin(x),
        color="tab:blue",      # line color
        linewidth=2.5,         # thickness
        linestyle="-",         # solid line; "--", ":", "-."
        marker="o",            # place a marker at each point
        markersize=4,
        label="sin(x)")
ax.plot(x, np.cos(x), color="tab:orange", linestyle="--", label="cos(x)")
plt.show()
```

A compact shorthand called a **format string** can set color, marker, and linestyle together:

```python
ax.plot(x, np.sin(x), "r--")      # red dashed line
```

### 1.5 Labels, title, legend, and grid

A plot without labels is a mystery. The OO methods are all of the form `ax.set_<thing>()`:

```python
fig, ax = plt.subplots()
ax.plot(x, np.sin(x), label="sin(x)")

ax.set_title("Sine wave")        # title above the axes
ax.set_xlabel("x (radians)")     # x-axis label
ax.set_ylabel("amplitude")       # y-axis label
ax.legend(loc="upper right")     # show the legend; a line needs a label=
ax.grid(True, linestyle=":", alpha=0.6)   # turn on a light grid
plt.show()
```

- A line only appears in the legend if it was given a `label=`.
- `legend(loc=...)` accepts names like `"upper right"`, `"lower left"`, `"best"`, or the numeric codes `0`–`10`.
- `grid(True)` toggles the grid; `axis="x"` or `axis="y"` restricts it to one direction.

### 1.6 Limits and axis scales

By default Matplotlib picks a range that fits your data plus a small margin. You can set it explicitly:

```python
ax.set_xlim(0, 2 * np.pi)
ax.set_ylim(-1.2, 1.2)
```

Sometimes a linear axis hides the interesting behavior. Matplotlib supports logarithmic, symmetric-log, logit, and other scales:

```python
x = np.linspace(0.1, 100, 200)

fig, ax = plt.subplots()
ax.plot(x, x ** 2, label="x²")
ax.set_xscale("log")     # logarithmic x-axis
ax.set_yscale("log")     # logarithmic y-axis
ax.set_xlabel("x (log scale)")
ax.set_ylabel("x² (log scale)")
ax.grid(True, which="both", linestyle=":", alpha=0.5)
plt.show()
```

- `set_xscale("log")` is equivalent to `plt.xscale("log")` in the state-machine API.
- On a log axis, straight lines correspond to power laws — this is why log axes are common in science and engineering.

### 1.7 Saving a figure with `savefig`

`fig.savefig()` writes the current figure to a file. The extension determines the format (`.png`, `.jpg`, `.svg`, `.pdf`).

```python
fig, ax = plt.subplots()
ax.plot(x, x ** 2)
fig.savefig("quadratic.png", dpi=150, bbox_inches="tight")
```

- `dpi=150` produces a higher-resolution raster image than the default (100).
- `bbox_inches="tight"` crops surrounding whitespace so labels are not clipped.
- With vector formats such as `.svg` and `.pdf`, `dpi` matters less because the image is resolution-independent.

> **Best practice note:** save *before* calling `plt.show()` in scripts, because some backends clear the figure when the window closes.

---

## ⚙️ Why This Matters

- **Reproducible figures in reports and papers:** setting `figsize`, `dpi`, and `bbox_inches` is exactly how you produce publication-quality images at the right size.
- **Reusable plotting code:** the OO API lets you write a function `make_plot(data) -> (fig, ax)` that you can call from notebooks, scripts, or web apps.
- **A foundation for everything else:** every later session — plot types, subplots, dashboards — builds on the `Figure`/`Axes` objects you learn here.
- **Debugging confidence:** knowing which object owns a property (figure vs. axes) tells you immediately where to look when a plot misbehaves.

---

## 🔍 Key Insights

- A `Figure` is the whole page; an `Axes` is one plot panel. One figure can hold many axes.
- The OO API (`fig, ax = plt.subplots(); ax.plot(...)`) is explicit and scales; the pyplot state machine (`plt.plot(...)`) is a convenience shortcut.
- `ax.set_*()` methods configure the axes; `fig.savefig()` saves the figure. Mixing these up is a common beginner error (`ax.savefig` does not exist).
- A line appears in the legend only if it has a `label=`; call `ax.legend()` afterward to actually draw it.
- `xlim`/`ylim` set what part of the data is *visible*, not what data exists — data outside the limits is still computed.
- `bbox_inches="tight"` is the single most useful `savefig` option, because it prevents clipped axis labels.
- `plt.show()` displays the figure; in a notebook it is optional because the figure renders inline automatically.

---

## 📦 Summary

Matplotlib draws **Artists** onto a **Figure**, and each plot panel inside that figure is an **Axes**. You create them together with `plt.subplots()`, draw into an axes with `ax.plot()`, and style it with `set_title`, `set_xlabel`, `set_ylabel`, `legend`, and `grid`. Visible data ranges are controlled with `set_xlim`/`set_ylim`, and axes can be rescaled with `set_xscale`/`set_yscale`. Finally, `fig.savefig("name.png", dpi=150, bbox_inches="tight")` writes a clean, high-resolution image you can drop into any document.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Import | `import matplotlib.pyplot as plt` |
| Figure + axes | `fig, ax = plt.subplots(figsize=(8, 4))` |
| Line plot | `ax.plot(x, y, color="tab:blue", linewidth=2)` |
| One-line style | `ax.plot(x, y, "r--o")` |
| Title / labels | `ax.set_title(...)`, `ax.set_xlabel(...)`, `ax.set_ylabel(...)` |
| Legend | `ax.plot(x, y, label="y"); ax.legend(loc="upper right")` |
| Grid | `ax.grid(True, linestyle=":", alpha=0.6)` |
| Limits | `ax.set_xlim(0, 10)`, `ax.set_ylim(-1, 1)` |
| Axis scale | `ax.set_xscale("log")`, `ax.set_yscale("log")` |
| Save | `fig.savefig("plot.png", dpi=150, bbox_inches="tight")` |
| Show | `plt.show()` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-02-plot-types/README.md">Session 2 ➡️</a>
</p>
