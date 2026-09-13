# 📘 Session 4 — Styling & Colormaps

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">📊 Matplotlib Track</a> ·
  <a href="../session-03-subplots-layouts-annotations/README.md">⬅️ Session 3</a> ·
  <a href="../session-05-data-storytelling-dashboard/README.md">Session 5 ➡️</a>
</p>

> **Goal:** take a chart from "default and forgettable" to "clear and polished" using colors, alpha, line styles, markers, `rcParams`, style sheets, and colormaps for images and heatmaps.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment + Mini Project sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Specify colors in all the ways Matplotlib accepts: named colors, hex codes, RGBA tuples, and the `'C0'`–`'C9'` cycle.
- Control transparency with `alpha` and understand how it reveals overlapping data.
- Style lines and points with `linestyle`, `marker`, `linewidth`, and `markersize`.
- Set global defaults once with `plt.rcParams`, `plt.rcParams.update(...)`, and `plt.style.use(...)`.
- Choose an appropriate **colormap** for sequential, diverging, and categorical data.
- Render 2D arrays as images with `imshow` and as heatmaps with `pcolormesh`, and attach a `colorbar`.

---

## 🧠 Concept Explanation

### 4.1 Colors: named, hex, RGBA, and the color cycle

**Intuition first.** A chart is a visual sentence, and color is the emphasis. Matplotlib gives you a surprisingly rich vocabulary for color — from a plain English word to a precise 4-number tuple with transparency.

**Formal explanation.** Matplotlib accepts colors in several equivalent formats:

| Format | Example | Notes |
|---|---|---|
| Named color | `'red'`, `'steelblue'`, `'tab:green'` | ~150 CSS names plus the `tab:` palette |
| Single-letter | `'r'`, `'g'`, `'b'`, `'k'` | Shortcuts for red, green, blue, black |
| Hex string | `'#1f77b4'` | `#RRGGBB`, values `00`–`ff` |
| Hex with alpha | `'#1f77b480'` | Optional trailing `80` = 50% opacity |
| RGB / RGBA tuple | `(0.1, 0.4, 0.9)`, `(0.1, 0.4, 0.9, 0.5)` | Floats in `[0, 1]` |
| Cycle shortcut | `'C0'`, `'C1'`, … `'C9'` | The default color cycle positions |

If you pass no color at all, Matplotlib walks through its **color cycle** — the sequence `C0, C1, C2, …` — one color per new artist.

```python
fig, ax = plt.subplots()
ax.plot([0, 1, 2], [0, 1, 4], color="steelblue")      # named
ax.plot([0, 1, 2], [0, 2, 3], color="#e63946")        # hex
ax.plot([0, 1, 2], [0, 3, 2], color=(0.2, 0.6, 0.3, 0.9))  # RGBA
ax.set_title("Three ways to say 'color'")
plt.show()
```

### 4.2 Transparency with `alpha`

`alpha` is a number from `0.0` (fully transparent) to `1.0` (fully opaque). Its real purpose is not decoration — it is **density**. When many points or lines overlap, a low `alpha` lets you see how concentrated the overlap is.

```python
rng = np.random.default_rng(0)
x, y = rng.normal(size=500), rng.normal(size=500)

fig, ax = plt.subplots()
ax.scatter(x, y, alpha=0.3, color="C0")
ax.set_title("alpha=0.3 reveals density where points overlap")
plt.show()
```

You can also set alpha per call (`ax.plot(..., alpha=0.6)`) or baked into an RGBA color tuple.

### 4.3 Linestyles, markers, and linewidth

A line has three visual dials: **how it is drawn** (`linestyle`), **what sits on the data points** (`marker`), and **how thick it is** (`linewidth`).

| Argument | Common values |
|---|---|
| `linestyle` | `'-'` solid, `'--'` dashed, `'-.'` dash-dot, `':'` dotted, `'none'` |
| `marker` | `'o'` circle, `'s'` square, `'^'` triangle, `'D'` diamond, `'*'` star, `'x'`, `'+'`, `'.'` |
| `linewidth` | Any positive float, e.g. `2.5` (points) |
| `markersize` | Any positive float, e.g. `8` |
| `markeredgecolor` | A color, often `'white'` for contrast |

```python
fig, ax = plt.subplots()
ax.plot([0, 1, 2, 3], [0, 1, 4, 9], linestyle="--", marker="o",
        linewidth=2, markersize=8, color="C2")
ax.set_title("Dashed line, circle markers, thick stroke")
plt.show()
```

A compact shortcut exists too: `ax.plot(x, y, "o--")` combines marker + linestyle in one string, but naming each argument is far more readable.

### 4.4 Global defaults with `rcParams`

**Intuition first.** Retheming every chart by hand is tedious. `rcParams` is a single dictionary of defaults that every new figure reads.

```python
plt.rcParams["figure.figsize"] = (8, 5)
plt.rcParams["figure.dpi"] = 120
plt.rcParams["font.size"] = 12
plt.rcParams["axes.grid"] = True
plt.rcParams["axes.spines.top"] = False
plt.rcParams["axes.spines.right"] = False
```

For several settings at once, prefer `plt.rcParams.update({...})` with a dictionary. To undo everything and return to the factory defaults, call `plt.rcdefaults()`.

```python
plt.rcParams.update({
    "figure.figsize": (8, 5),
    "axes.titlesize": 14,
    "axes.titleweight": "bold",
    "grid.alpha": 0.3,
})
```

> **Best practice note:** set `rcParams` at the *top* of a notebook, before you create figures, and reset with `plt.rcdefaults()` when you deliberately change them for a single experiment.

### 4.5 Built-in style sheets

Matplotlib ships with ready-made themes. Inspect them with `plt.style.available`, then apply one globally with `plt.style.use("ggplot")` or locally with a context manager:

```python
with plt.style.context("dark_background"):
    fig, ax = plt.subplots()
    ax.plot([0, 1, 2], [0, 1, 4])
    plt.show()
# outside the with-block, the previous style is restored automatically
```

The context manager is the safest habit: it scopes the theme to a single figure and never leaks into the rest of your notebook.

### 4.6 Choosing a colormap

A **colormap** (or `cmap`) maps numbers to colors. Choosing the right family matters more than choosing a pretty one:

- **Sequential** — low to high, one hue getting darker/brighter. Use for magnitudes: `'viridis'`, `'plasma'`, `'cividis'`, `'Blues'`, `'magma'`.
- **Diverging** — a meaningful center (often zero) with two opposite hues. Use for signed data: `'coolwarm'`, `'RdBu'`, `'seismic'`.
- **Cyclic** — wraps around, good for angles/phases: `'twilight'`, `'hsv'`.
- **Qualitative** — distinct unordered categories: `'tab10'`, `'Set2'`, `'Paired'`.

> **Accessibility note:** `'viridis'`, `'plasma'`, and `'cividis'` are *perceptually uniform* and color-blind friendly — safe defaults for continuous data.

### 4.7 Displaying matrices: `imshow` and `colorbar`

`imshow` treats a 2D array (or a 3D `(rows, cols, 3)` array) as an image. Every number in the array becomes a pixel, colored by the colormap.

```python
Z = np.arange(100).reshape(10, 10)

fig, ax = plt.subplots()
im = ax.imshow(Z, cmap="viridis", origin="lower")
fig.colorbar(im, ax=ax, label="value")
ax.set_title("imshow with a labeled colorbar")
plt.show()
```

Key arguments: `origin="lower"` puts row 0 at the bottom (matching math axes), `extent=[x0, x1, y0, y1]` attaches physical coordinates, and `aspect="auto"` lets the cells stretch to fill the axes. The returned object `im` is a `ScalarMappable`; passing it to `fig.colorbar(im, ax=ax)` draws the legend that translates colors back into numbers.

### 4.8 Heatmaps with `pcolormesh`

`pcolormesh` draws a colored grid of quadrilaterals. Unlike `imshow`, it accepts **explicit x/y coordinates** and supports **non-uniform grids** — ideal for real heatmaps.

```python
x = np.linspace(0, 5, 30)
y = np.linspace(0, 5, 20)
X, Y = np.meshgrid(x, y)
Z = np.sin(X) * np.cos(Y)

fig, ax = plt.subplots()
mesh = ax.pcolormesh(X, Y, Z, cmap="coolwarm", shading="auto")
fig.colorbar(mesh, ax=ax, label="sin(x)·cos(y)")
ax.set_title("pcolormesh heatmap")
plt.show()
```

> **Rule of thumb:** use `imshow` when you have a plain matrix or an actual image; use `pcolormesh` when the x/y coordinates are irregular or when you need true data coordinates on the axes.

---

## ⚙️ Why This Matters

- **Communication:** two charts can carry identical data but only one earns trust. Styling is what turns analysis into a message.
- **Accessibility:** perceptually uniform, color-blind-friendly colormaps keep your work readable for everyone.
- **Consistency:** `rcParams` and style sheets give a whole report one coherent look with a few lines at the top.
- **Density and overlap:** `alpha` is the standard way to visualize thousands of overlapping points without hiding the structure.
- **Scientific imaging:** `imshow`, `pcolormesh`, and `colorbar` are the backbone of heatmaps, spectrograms, correlation matrices, and satellite imagery.

---

## 🔍 Key Insights

- Matplotlib has **one color model with many syntaxes** — named, hex, RGBA tuple, and `'C0'` all end up in the same place.
- `alpha` is a *data-density* tool first and a decoration second.
- `rcParams` defaults apply to **new** figures; a figure already created keeps the settings it was born with.
- `plt.style.context(...)` is the leak-proof way to try a theme, whereas `plt.style.use(...)` changes global state.
- A colormap family must match the *meaning* of the data: sequential for magnitude, diverging for signed values, qualitative for categories.
- `imshow` is for regular rasters; `pcolormesh` is for grids with real, possibly uneven coordinates.
- `fig.colorbar(im, ax=ax)` needs the *returned* artist; a bare `plt.colorbar()` relies on the current image and is easy to misattach.

---

## 📦 Summary

Styling is the craft layer of Matplotlib. You can express a color as a name, a hex string, an RGBA tuple, or a cycle shortcut; you control transparency with `alpha`; and you shape lines and points with `linestyle`, `marker`, and `linewidth`. Rather than styling every chart by hand, you set defaults once through `plt.rcParams` and `plt.rcParams.update(...)`, or adopt a whole theme with `plt.style.use(...)` / `plt.style.context(...)`. Continuous data becomes an image through `imshow` or a heatmap through `pcolormesh`, and a `colorbar` turns colors back into interpretable numbers. Choosing the correct colormap family — sequential, diverging, cyclic, or qualitative — is what makes the result both correct and accessible.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Named / hex / RGBA color | `color="steelblue"`, `color="#e63946"`, `color=(0.2, 0.6, 0.3, 0.9)` |
| Cycle colors | `color="C0"` … `color="C9"` |
| Transparency | `alpha=0.3` |
| Line style / marker / width | `linestyle="--"`, `marker="o"`, `linewidth=2` |
| Set one default | `plt.rcParams["axes.grid"] = True` |
| Set many defaults | `plt.rcParams.update({...})` |
| Reset defaults | `plt.rcdefaults()` |
| Available themes | `plt.style.available` |
| Apply theme (global) | `plt.style.use("ggplot")` |
| Apply theme (scoped) | `with plt.style.context("dark_background"): ...` |
| Matrix as image | `ax.imshow(Z, cmap="viridis", origin="lower")` |
| Colormap legend | `fig.colorbar(im, ax=ax, label="value")` |
| Heatmap with real coords | `ax.pcolormesh(X, Y, Z, cmap="coolwarm", shading="auto")` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-05-data-storytelling-dashboard/README.md">Session 5 ➡️</a>
</p>
