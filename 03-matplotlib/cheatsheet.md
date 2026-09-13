# 📄 Matplotlib Cheat Sheet

A one-page reference for everyday plotting. Every snippet assumes:

```python
import matplotlib.pyplot as plt
import numpy as np
```

> Prefer the **object-oriented** style: `fig, ax = plt.subplots()` then call `ax.*`.

---

## 1. The core template

```python
fig, ax = plt.subplots(figsize=(8, 5))
ax.plot(x, y)
ax.set(title="Title", xlabel="x", ylabel="y")
ax.legend()
ax.grid(alpha=0.3)
fig.savefig("plot.png", dpi=150, bbox_inches="tight")
plt.show()
```

## 2. Line plots

```python
x = np.linspace(0, 10, 200)
ax.plot(x, np.sin(x), color="tab:blue", lw=2, ls="-", label="sin")
ax.plot(x, np.cos(x), color="tab:red", lw=2, ls="--", label="cos")
ax.plot(x, x**2, marker="o", markersize=3, label="x^2")
```

## 3. Common chart types

```python
ax.scatter(x, y, s=40, c="tab:green", alpha=0.7)     # relationship
ax.bar(["A", "B", "C"], [3, 7, 5])                   # category comparison
ax.barh(["A", "B", "C"], [3, 7, 5])                  # horizontal bar
ax.hist(data, bins=20, edgecolor="white")            # distribution
ax.boxplot([g1, g2, g3], tick_labels=["g1", "g2", "g3"])  # spread & outliers
ax.errorbar(x, y, yerr=err, fmt="o", capsize=4)      # uncertainty
ax.fill_between(x, y - err, y + err, alpha=0.3)      # shaded band
ax.pie([30, 45, 25], labels=["A", "B", "C"], autopct="%1.0f%%")
```

## 4. Labels, title, legend, grid

```python
ax.set_title("Main title", fontsize=14, weight="bold")
ax.set_xlabel("Time (s)")
ax.set_ylabel("Value")
ax.legend(loc="upper right", frameon=False)
ax.grid(True, alpha=0.3)
```

## 5. Limits, ticks & scales

```python
ax.set_xlim(0, 10)
ax.set_ylim(-2, 2)
ax.set_xticks([0, 2, 4, 6, 8, 10])
ax.set_yscale("log")          # 'linear' | 'log' | 'symlog'
ax.invert_yaxis()
ax.tick_params(axis="x", rotation=45)
```

## 6. Multiple subplots & layout

```python
fig, axes = plt.subplots(2, 2, figsize=(10, 8), sharex=True)
axes[0, 0].plot(x, y1)
axes[0, 1].scatter(x, y2)
axes[1, 0].hist(y3, bins=15)
axes[1, 1].bar(cats, vals)

fig, axes = plt.subplots(1, 2, layout="constrained")   # modern spacing
fig.suptitle("Dashboard")
```

```python
from matplotlib.gridspec import GridSpec
gs = GridSpec(2, 3, figure=fig)
ax_a = fig.add_subplot(gs[0, :])    # span the full top row
ax_b = fig.add_subplot(gs[1, 0])
```

## 7. Twin axes (two y-scales)

```python
fig, ax1 = plt.subplots()
ax2 = ax1.twinx()
ax1.plot(x, temperature, color="tab:red")
ax2.plot(x, rainfall, color="tab:blue")
ax1.set_ylabel("Temp", color="tab:red")
ax2.set_ylabel("Rain", color="tab:blue")
```

## 8. Annotations

```python
ax.annotate("peak", xy=(x0, y0), xytext=(x0 + 1, y0 + 1),
            arrowprops=dict(arrowstyle="->"))
ax.text(x0, y0, "note", fontsize=9)
ax.axhline(y=0, color="gray", ls=":")
ax.axvline(x=5, color="gray", ls=":")
ax.axvspan(3, 5, color="yellow", alpha=0.2)
```

## 9. Styling

```python
plt.style.use("seaborn-v0_8-whitegrid")   # style sheet
# named colors, hex, or RGBA
ax.plot(x, y, color="#ff7f0e", alpha=0.6, lw=2, ls="-.", marker="s")

# custom rcParams
plt.rcParams.update({"figure.dpi": 110, "font.size": 11})
```

## 10. Colormaps, images, colorbars

```python
cmap = plt.get_cmap("viridis")
sc = ax.scatter(x, y, c=values, cmap="viridis")   # map data -> color
fig.colorbar(sc, ax=ax, label="value")

im = ax.imshow(matrix, cmap="magma", aspect="auto")  # 2D array -> image
fig.colorbar(im, ax=ax)

ax.pcolormesh(X, Y, Z, cmap="coolwarm", shading="auto")  # heatmap
```

## 11. Saving

```python
fig.savefig("figure.png", dpi=300, bbox_inches="tight", transparent=False)
fig.savefig("figure.svg")   # vector format for publications
```

---

## ⚠️ Top pitfalls

1. Use `fig, ax = plt.subplots()`; avoid mixing the pyplot state machine with `ax.*`.
2. Call `ax.set(...)` (not `ax.title(...)`/`ax.xlabel(...)`).
3. With multiple figures, always target axes explicitly (`axes[i].plot(...)`).
4. `plt.show()` in a notebook needs `%matplotlib inline`; save with `fig.savefig` for files.
5. `sharex=True`/`sharey=True` prevents repeated tick labels on subplot grids.
6. Accessibility: don't rely on color alone; add labels, markers, or patterns.

---

<div align="center"><a href="./README.md">🏠 Back to the track</a> · <a href="../README.md">🧪 Lab home</a></div>
