# 📘 Session 5 — Data Storytelling & Dashboards

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">📊 Matplotlib Track</a> ·
  <a href="../session-04-styling-and-colormaps/README.md">⬅️ Session 4</a>
</p>

> **Goal:** turn analysis into a story — combine pandas with Matplotlib, assemble multi-panel dashboards, guide the reader with annotations and captions, follow visualization best practices, and export publication-quality figures.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment + Mini Project sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Build a pandas `DataFrame` and plot its columns directly with Matplotlib's object-oriented API.
- Compose multi-panel figures with `plt.subplots(...)` and reason about which panels share a story.
- Use `sharex` / `sharey` to align axes and remove redundant tick labels.
- Highlight findings with `ax.annotate`, reference lines, and shaded spans.
- Layer a title, subtitles, and a source caption so a figure explains itself.
- Apply visualization best practices and export the result with `savefig` at publication quality.

---

## 🧠 Concept Explanation

### 5.1 Why pandas + Matplotlib together?

**Intuition first.** pandas is good at *organizing* data (columns, labels, groupings). Matplotlib is good at *drawing* it. A data story usually starts in pandas — filter, group, pivot — and ends in Matplotlib, where you control exactly how the numbers become a picture.

**Formal explanation.** A `DataFrame` can be handed to Matplotlib in two idiomatic ways:

```python
fig, ax = plt.subplots()

# (a) plot a Series directly
ax.plot(df["month"], df["revenue"])

# (b) let pandas draw onto the Axes you created
df.plot(x="month", y="revenue", ax=ax)
```

Both are valid. This course prefers creating `fig, ax` yourself and passing `ax=ax` to pandas, because **you** keep control of the figure, the panels, and the styling.

For a wide table (one column per product), pandas can draw every column at once:

```python
pivot.plot(ax=ax, marker="o")
```

### 5.2 Multi-panel figures

A dashboard is a **grid of small stories**. `plt.subplots(rows, cols)` returns a figure plus an array of axes:

```python
fig, axes = plt.subplots(2, 2, figsize=(13, 8))
axes[0, 0].plot(...)
axes[0, 1].bar(...)
axes[1, 0].scatter(...)
axes[1, 1].imshow(...)
```

Two layout engines keep panels from colliding:

- `fig.tight_layout()` — squeezes the current layout after you plot. Use `fig.tight_layout(rect=[0, 0.03, 1, 0.95])` to reserve room for a title or caption.
- `constrained_layout=True` — pass it to `plt.subplots(...)` and let Matplotlib negotiate spacing automatically.

> **Rule of thumb:** with nested titles and captions, `tight_layout(rect=...)` gives the most predictable control.

### 5.3 Shared axes

When two panels describe the same x-axis (for example, months), aligning them makes comparison effortless:

```python
fig, axes = plt.subplots(2, 1, sharex=True, figsize=(9, 6))
```

`sharex=True` links the x-limits and hides the inner tick labels, removing duplication. `sharey=True` does the same vertically. Only share an axis when the units and scale genuinely match — sharing a date axis with a categorical axis is a common mistake.

### 5.4 Annotations: point at the finding

A dashboard should not just show data; it should tell the reader *what to notice*. Matplotlib offers several tools:

| Tool | Purpose |
|---|---|
| `ax.annotate(text, xy=..., xytext=..., arrowprops=...)` | Label a specific point with an arrow |
| `ax.axvline(x=...)` / `ax.axhline(y=...)` | Draw a reference line (target, average, event date) |
| `ax.axhspan(y0, y1)` / `ax.axvspan(x0, x1)` | Shade a band (acceptable range, event window) |
| `ax.text(x, y, s)` | Free-floating text in data coordinates |

```python
peak_month = monthly_total.idxmax()
peak_value = monthly_total.max()

ax.annotate(
    f"Peak: {peak_value:,.0f}",
    xy=(peak_month, peak_value),
    xytext=(0, 25),
    textcoords="offset points",
    ha="center",
    arrowprops=dict(arrowstyle="->", color="black"),
)
```

Using `textcoords="offset points"` places the label a fixed distance from the point instead of in data units — much more predictable.

### 5.5 Titles, subtitles, and captions

A self-explaining figure has three layers of text:

```python
fig.suptitle("Sales Dashboard 2024", fontsize=16, fontweight="bold")   # the headline
ax.set_title("Revenue by product", loc="left")                          # each panel's title
ax.text(0, 1.02, "Monthly totals, USD", transform=ax.transAxes,         # a subtitle
        fontsize=10, color="gray")
fig.text(0.5, 0.005, "Source: synthetic data generated in this notebook",
         ha="center", fontsize=8, color="gray")                         # the caption
```

Key idea: `ax.text(0, 1.02, ..., transform=ax.transAxes)` uses **axes coordinates**, so `y=1.02` sits just above the panel regardless of the data scale.

### 5.6 Visualization best practices

Good charts follow a small set of rules that consistently improve clarity:

- **Label everything:** axis titles, units, and a legend when more than one series is drawn.
- **Start bar charts at zero:** truncated bars exaggerate differences and mislead.
- **Reduce chartjunk:** drop unnecessary gridlines, borders, and background shading.
- **Order categories meaningfully:** sort bars by value unless the order itself carries meaning (e.g. months).
- **Use color with intent:** highlight the series that matters and mute the rest; prefer color-blind-friendly palettes.
- **One message per panel:** if a panel needs three sentences to explain, split it.
- **Avoid dual y-axes** unless unavoidable — they invite false correlations.
- **Be honest about uncertainty:** show ranges or error bars when they exist.

### 5.7 Saving at publication quality

`savefig` is the final step. The three settings that matter most:

```python
fig.savefig("dashboard.png", dpi=300, bbox_inches="tight", facecolor=fig.get_facecolor())
```

- **`dpi=300`** — print-quality raster resolution (use `>= 600` for line art).
- **`bbox_inches="tight"`** — trims the whitespace around the figure.
- **Vector formats** — `.pdf` or `.svg` stay sharp at any zoom and are ideal for papers and slides. Add `transparent=True` for slides with a colored background.

> **Tip:** save *before* you call `plt.show()`. Once a figure is closed, `savefig` can no longer reach it.

---

## ⚙️ Why This Matters

- **Decision-making:** a dashboard compresses weeks of numbers into a few seconds of reading — the core deliverable of most data roles.
- **Trust:** honest axes, clear labels, and cited sources are what separate analysis from advertising.
- **Reuse:** the pandas → `fig, ax` → `savefig` pipeline scales from a quick exploratory chart to a printed report without rewriting anything.
- **Career leverage:** "can build a clear dashboard" is one of the most requested and most visible skills in analytics, finance, and product work.

---

## 🔍 Key Insights

- pandas **organizes**, Matplotlib **explains**. Keep the responsibilities separate and pass `ax=ax` explicitly.
- `plt.subplots( rows, cols)` returns an array of axes; index it like `axes[0, 1]`.
- Share axes only when units and scale truly match — otherwise you hide real differences.
- `ax.annotate` with `textcoords="offset points"` is the most predictable way to label a point.
- `suptitle` (figure) + `set_title` (panel) + `fig.text` (caption) form the three text layers of a story.
- `bbox_inches="tight"` plus a high `dpi` (or a vector format) is what makes a figure print-ready.
- A chart that needs a paragraph to explain is usually two charts.

---

## 📦 Summary

Data storytelling is the discipline of turning a pandas analysis into a figure a stranger can read. You plot columns directly with `ax.plot(series)` or `df.plot(ax=ax)`, arrange several views in a `plt.subplots(...)` grid, and link related panels with `sharex` / `sharey`. You point the reader at the finding with `ax.annotate`, reference lines, and shaded spans; you orient them with a `suptitle`, per-panel titles, subtitles, and a source caption; and you respect the best practices of honest axes, purposeful color, and one message per panel. Finally, `fig.savefig(..., dpi=300, bbox_inches="tight")` — or a `.pdf` / `.svg` — delivers the result at publication quality.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Plot a DataFrame column | `ax.plot(df["month"], df["revenue"])` |
| Let pandas draw on your Axes | `df.plot(x="month", y="revenue", ax=ax)` |
| Plot every column of a wide table | `wide_df.plot(ax=ax)` |
| Multi-panel grid | `fig, axes = plt.subplots(2, 2, figsize=(13, 8))` |
| Shared x / y | `plt.subplots(2, 1, sharex=True)` |
| Automatic spacing | `constrained_layout=True` or `fig.tight_layout(rect=[...])` |
| Annotate a point | `ax.annotate("Peak", xy=(x, y), xytext=(0, 20), textcoords="offset points", arrowprops=dict(arrowstyle="->"))` |
| Reference line / band | `ax.axvline(x=...)`, `ax.axhspan(y0, y1, alpha=0.15)` |
| Figure headline | `fig.suptitle("...", fontweight="bold")` |
| Panel title / subtitle | `ax.set_title("...", loc="left")`, `ax.text(0, 1.02, "...", transform=ax.transAxes)` |
| Source caption | `fig.text(0.5, 0.005, "Source: ...", ha="center", fontsize=8)` |
| Export print quality | `fig.savefig("plot.png", dpi=300, bbox_inches="tight")` |
| Export vector | `fig.savefig("plot.pdf")` / `fig.savefig("plot.svg")` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="./exercises_solutions.ipynb">✅ Check the solutions</a>
</p>
