# 📘 Session 2 — Plot Types & Choosing the Right Chart

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../README.md">📊 Matplotlib Track</a> ·
  <a href="../session-01-figure-axes-basics/README.md">⬅️ Session 1</a> ·
  <a href="../session-03-subplots-layouts-annotations/README.md">Session 3 ➡️</a>
</p>

> **Goal:** use Matplotlib's core plot types — scatter, bar, histogram, boxplot, pie, errorbar, fill_between, and stem — and choose the right one for a given dataset.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Pick the appropriate chart type for a question about relationships, categories, distributions, or change over time.
- Draw relationships with `ax.scatter()` and categorical comparisons with `ax.bar()` / `ax.barh()`.
- Show a distribution with `ax.hist()` and compare distributions with `ax.boxplot()`.
- Show composition with `ax.pie()` and uncertainty with `ax.errorbar()`.
- Emphasize a range or an area with `ax.fill_between()` and discrete signals with `ax.stem()`.
- Read the signature and key keyword arguments of each function from its documentation.

---

## 🧠 Concept Explanation

### 2.1 Start with the question, not the chart

**Intuition first.** A chart is an answer to a question. Before writing any code, ask *what am I trying to show?* The answer usually falls into one of five buckets, and each bucket has a natural chart family:

| Question | Bucket | Go-to chart |
|---|---|---|
| Do x and y move together? | Relationship | `scatter` |
| Which category is biggest? | Comparison | `bar` / `barh` |
| How is one variable spread out? | Distribution | `hist` / `boxplot` |
| What share does each part hold? | Composition | `pie` |
| How uncertain is this measurement? | Uncertainty | `errorbar` |

**Formal explanation.** Matplotlib exposes each of these as a method on `Axes`. They all share the same object model from Session 1: create `fig, ax = plt.subplots()`, call the method, style it, then show or save.

### 2.2 Relationships: `ax.scatter()`

A scatter plot places one dot per observation at `(x, y)`. It is the standard tool for spotting correlation, clusters, and outliers.

```python
rng = np.random.default_rng(0)
x = rng.normal(size=200)
y = 2 * x + rng.normal(scale=0.5, size=200)

fig, ax = plt.subplots()
ax.scatter(x, y, s=30, c="tab:blue", alpha=0.6, edgecolors="white")
ax.set_title("Relationship between x and y")
ax.set_xlabel("x")
ax.set_ylabel("y")
plt.show()
```

- `s` is the marker **area** in points².
- `c` sets the color; it can be a single color or an array mapped through a colormap.
- `alpha` (0–1) adds transparency so overlapping points remain readable.

### 2.3 Comparison: `ax.bar()` and `ax.barh()`

Bar charts compare a numeric value across categories. `bar` draws vertical bars, `barh` draws horizontal bars (useful for long category names).

```python
categories = ["A", "B", "C", "D"]
values = [23, 45, 12, 36]

fig, ax = plt.subplots()
ax.bar(categories, values, color="tab:green", edgecolor="black")
ax.set_title("Values by category")
ax.set_ylabel("value")
plt.show()
```

- A grouped bar chart is two `bar` calls, offsetting the x positions.
- A stacked bar chart is two `bar` calls using the `bottom=` argument.

### 2.4 Distribution: `ax.hist()` and `ax.boxplot()`

A histogram bins the data and draws the count (or density) per bin:

```python
rng = np.random.default_rng(1)
samples = rng.normal(loc=0, scale=1, size=1000)

fig, ax = plt.subplots()
ax.hist(samples, bins=30, density=True, color="tab:blue", alpha=0.75)
ax.set_title("Histogram of normal samples")
ax.set_xlabel("value")
ax.set_ylabel("density")
plt.show()
```

- `bins` can be an integer or an explicit array of bin edges.
- `density=True` normalizes the area to 1 so the shape can be compared to a PDF.

A box plot summarizes a distribution with its median, quartiles, and whiskers, and flags outliers:

```python
data = [rng.normal(0, 1, 200), rng.normal(2, 0.5, 200)]

fig, ax = plt.subplots()
ax.boxplot(data, tick_labels=["group A", "group B"])
ax.set_title("Distribution by group")
ax.set_ylabel("value")
plt.show()
```

> In Matplotlib 3.9+ the parameter for box labels is `tick_labels`. On older versions it is `labels`; on Matplotlib 3.11 use `tick_labels`.

### 2.5 Composition: `ax.pie()`

A pie chart shows how a whole is divided into parts. Use it only when there are a handful of categories that sum to a meaningful whole.

```python
sizes = [35, 25, 20, 20]
labels = ["A", "B", "C", "D"]

fig, ax = plt.subplots()
ax.pie(sizes, labels=labels, autopct="%1.1f%%", startangle=90)
ax.set_title("Composition")
ax.axis("equal")     # keeps the pie circular
plt.show()
```

- `autopct` formats the percentage labels.
- `ax.axis("equal")` forces equal scaling so the circle is not stretched into an ellipse.

### 2.6 Uncertainty: `ax.errorbar()`

Error bars communicate measurement uncertainty or variability. `errorbar` draws the points plus the bars in one call:

```python
x = np.arange(6)
y = np.array([1.0, 2.1, 2.9, 4.2, 5.0, 6.1])
yerr = np.array([0.3, 0.2, 0.4, 0.3, 0.2, 0.5])

fig, ax = plt.subplots()
ax.errorbar(x, y, yerr=yerr, fmt="o-", capsize=5, color="tab:red")
ax.set_title("Measurement with uncertainty")
ax.set_xlabel("trial")
ax.set_ylabel("value")
plt.show()
```

- `fmt` combines marker and line style, exactly like `plot`'s format string.
- `yerr` (and/or `xerr`) can be a scalar, an array, or a pair of arrays for asymmetric errors.

### 2.7 Range and shape: `ax.fill_between()` and `ax.stem()`

`fill_between` shades the area between two curves — perfect for uncertainty bands or highlighting a region:

```python
x = np.linspace(0, 2 * np.pi, 200)
mean = np.sin(x)
band = 0.2

fig, ax = plt.subplots()
ax.plot(x, mean, color="tab:blue", label="mean")
ax.fill_between(x, mean - band, mean + band, color="tab:blue", alpha=0.25,
                label="±0.2 band")
ax.set_title("Mean with an uncertainty band")
ax.legend()
plt.show()
```

`stem` draws a vertical line from a baseline to each point, ideal for discrete sequences such as digital signals:

```python
n = np.arange(12)
signal = np.array([0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0])

fig, ax = plt.subplots()
ax.stem(n, signal)
ax.set_title("Discrete signal")
ax.set_xlabel("sample")
ax.set_ylabel("level")
plt.show()
```

### 2.8 Choosing the right chart

**Intuition first.** If you remember nothing else, remember this: *relationships → scatter, comparisons → bar, distributions → histogram or box, parts of a whole → pie (sparingly), uncertainty → errorbar, shaded ranges → fill_between, discrete sequences → stem.*

**Formal explanation.** Chart choice is a mapping from **data shape + question** to **visual encoding**:

- Continuous x and continuous y → **scatter**.
- Categorical x and numeric y → **bar** / **barh**.
- One numeric variable, many observations → **hist**; several groups → **boxplot**.
- Parts summing to a whole → **pie** (be cautious; bar charts are often clearer).
- Estimates with error → **errorbar**.
- A lower and an upper bound → **fill_between**.
- A sequence indexed by discrete steps → **stem**.

---

## ⚙️ Why This Matters

- **Honest communication:** the wrong chart hides the signal or invents one; the right chart makes the pattern obvious.
- **Exploratory data analysis:** scatter, histogram, and boxplot are the first three plots a data scientist runs on any new dataset.
- **Reports and dashboards:** bar and line charts are the workhorses of business reporting; error bars are expected in scientific results.
- **A shared vocabulary:** knowing the names means you can ask for (or search for) the exact chart you need.

---

## 🔍 Key Insights

- Choose the chart from the question, not from habit — the same dataset supports several valid answers.
- `scatter` never connects points; `plot` does. Use `scatter` when order does not matter.
- `density=True` in `hist` makes the area sum to 1, which is what lets you compare shapes across different sample sizes.
- Error bars encode *uncertainty*, not decoration — always say in the caption what `yerr` represents.
- Pie charts break down with many slices or near-equal shares; a horizontal bar chart is usually more readable.
- `fill_between` between a mean and its confidence band is a compact way to show spreading uncertainty.
- `stem` is the discrete cousin of `plot`: it emphasizes that the x-values are separate samples, not a continuous curve.

---

## 📦 Summary

Matplotlib offers a method for every common data question: `scatter` for relationships, `bar`/`barh` for category comparisons, `hist`/`boxplot` for distributions, `pie` for composition, `errorbar` for uncertainty, `fill_between` for shaded ranges, and `stem` for discrete sequences. All of them follow the same object-oriented pattern — create `fig, ax = plt.subplots()`, call the method, style the result, and display or save it. The real skill is not memorizing the calls; it is matching the chart to the question the data is meant to answer.

---

## ⚡ Quick Reference

| Goal | Call |
|---|---|
| Relationship | `ax.scatter(x, y, s=30, alpha=0.6)` |
| Category comparison | `ax.bar(cats, vals)` / `ax.barh(cats, vals)` |
| Distribution (binned) | `ax.hist(data, bins=30, density=True)` |
| Distribution (summary) | `ax.boxplot(data, tick_labels=names)` |
| Composition | `ax.pie(sizes, labels=labels, autopct="%1.1f%%")` |
| Uncertainty | `ax.errorbar(x, y, yerr=err, fmt="o-", capsize=5)` |
| Shaded range | `ax.fill_between(x, lo, hi, alpha=0.25)` |
| Discrete signal | `ax.stem(n, signal)` |
| Circular pie | `ax.axis("equal")` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-03-subplots-layouts-annotations/README.md">Session 3 ➡️</a>
</p>
