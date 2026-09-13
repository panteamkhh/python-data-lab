# 📊 Matplotlib Track

<p align="center">
  <a href="../README.md">🏠 Lab Home</a> ·
  <a href="./cheatsheet.md">📄 Matplotlib Cheat Sheet</a> ·
  <a href="../02-pandas/README.md">⬅️ Pandas Track</a>
</p>

> **Turn numbers into insight.** Matplotlib is the plotting engine behind pandas' `.plot()`, seaborn, and most scientific Python. This track takes you from your first line chart to fully styled, multi-panel dashboards.

---

## 🎯 What this track covers

Five sessions covering the Figure/Axes object model, every essential chart type, subplot layouts and annotations, styling with colors and colormaps, and finally data storytelling — assembling polished, publication-quality dashboards.

---

## 📚 Sessions

| # | Session | Core topics | Mini project |
|:-:|---|---|---|
| **01** | [Figure/Axes & First Plots](./session-01-figure-axes-basics/README.md) | Figure/Axes model, `plot`, labels, titles, legends, `savefig` | 📈 Plot a Function |
| **02** | [Plot Types](./session-02-plot-types/README.md) | `scatter`, `bar`, `hist`, `boxplot`, `pie`, `errorbar` | 📊 Distribution & Comparison |
| **03** | [Subplots, Layouts & Annotations](./session-03-subplots-layouts-annotations/README.md) | Grids, `sharex`/`sharey`, `GridSpec`, `twinx`, `annotate` | 🧩 2×2 Dashboard |
| **04** | [Styling & Colormaps](./session-04-styling-and-colormaps/README.md) | Colors, markers, `rcParams`, style sheets, colormaps, `imshow` | 🎨 Chart Makeover |
| **05** | [Data Storytelling & Dashboards](./session-05-data-storytelling-dashboard/README.md) | pandas + matplotlib, multi-panel figures, annotation, best practices | 📰 Sales Dashboard |

---

## 🧭 How to use this track

Each session has the same five files:

- `README.md` — theory (intuition → formal → code)
- `notebook.ipynb` — runnable **Build It** + **Experiment** cells (with inline plots)
- `exercises.md` — 5 graded exercises + a mini project
- `exercises_solutions.ipynb` — full worked solutions
- `ai-learning.md` — prompts to learn the session with an AI assistant

> **Prerequisite:** the [NumPy Track](../01-numpy/README.md) (for arrays) and ideally the [Pandas Track](../02-pandas/README.md) (session 5 plots DataFrames).

---

## ⚡ Quick reference

```python
import matplotlib.pyplot as plt
import numpy as np

fig, ax = plt.subplots(figsize=(8, 5))      # Figure + Axes (preferred style)
x = np.linspace(0, 2 * np.pi, 200)
ax.plot(x, np.sin(x), label="sin(x)", color="tab:blue", lw=2)
ax.set(title="A sine wave", xlabel="x", ylabel="y", xlim=(0, 6.3), ylim=(-1.2, 1.2))
ax.legend()
ax.grid(alpha=0.3)
fig.savefig("plot.png", dpi=150, bbox_inches="tight")   # save publication-quality
plt.show()
```

| Task | Call |
|---|---|
| New figure + axes | `fig, ax = plt.subplots()` |
| Line / scatter / bar | `ax.plot`, `ax.scatter`, `ax.bar` |
| Labels & title | `ax.set(xlabel=..., ylabel=..., title=...)` |
| Grid / legend | `ax.grid()`, `ax.legend()` |
| Subplots grid | `fig, axes = plt.subplots(2, 2)` |
| Twin y-axis | `ax2 = ax.twinx()` |
| Colorbar | `fig.colorbar(im, ax=ax)` |
| Save | `fig.savefig("out.png", dpi=300)` |

> Prefer a single page? See the [Matplotlib Cheat Sheet](./cheatsheet.md).

---

<p align="center">
  <a href="./cheatsheet.md">📄 Matplotlib Cheat Sheet</a> ·
  <a href="../02-pandas/README.md">⬅️ Pandas Track</a> ·
  <a href="../README.md">🏠 Lab Home</a>
</p>
