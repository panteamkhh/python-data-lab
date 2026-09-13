<div align="center">

# 🧪 Python Data Lab

### A hands-on, project-driven lab for the **Python data stack**: **NumPy · Pandas · Matplotlib**

**3 tracks · 15 sessions · 75 exercises · 15 mini projects · AI-assisted learning built in** 🚀

<p>
  <img alt="Python" src="https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white">
  <img alt="NumPy" src="https://img.shields.io/badge/NumPy-2.x-013243?logo=numpy&logoColor=white">
  <img alt="Pandas" src="https://img.shields.io/badge/Pandas-3.x-150458?logo=pandas&logoColor=white">
  <img alt="Matplotlib" src="https://img.shields.io/badge/Matplotlib-3.x-11557C?logo=matplotlib&logoColor=white">
  <img alt="Jupyter" src="https://img.shields.io/badge/Made%20with-Jupyter-F37626?logo=jupyter&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-success.svg">
  <img alt="PRs welcome" src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg">
</p>

</div>

---

## 🌟 Why this lab?

Most tutorials teach one library in isolation and stop at toy examples. This lab builds the **whole data workflow** — from raw numbers, to cleaned tabular data, to a story told in charts — with real (but self-contained) projects at every step.

- **Start from zero:** no prior experience required; nothing shallow either.
- **Learn by doing:** every session pairs theory (`README.md`) with runnable code (`notebook.ipynb`) and graded practice.
- **Real projects:** 15 mini projects, from a grayscale image converter to a multi-panel sales dashboard.
- **No downloads needed:** every dataset is generated inline, so everything runs out of the box.
- **AI-assisted:** each session ships an `ai-learning.md` with prompts, code review, and debugging drills.
- **Interview-ready:** views vs. copies, broadcasting, Copy-on-Write, `loc`/`iloc`, split–apply–combine, plot anatomy.

---

## 🗂️ The three tracks

| Track | Folder | You'll learn |
|:-:|---|---|
| 🔢 **NumPy** | [`01-numpy/`](./01-numpy/README.md) | Arrays, vectorization, indexing, broadcasting, images |
| 🐼 **Pandas** | [`02-pandas/`](./02-pandas/README.md) | Series/DataFrames, cleaning, groupby, merging, time series |
| 📊 **Matplotlib** | [`03-matplotlib/`](./03-matplotlib/README.md) | Figure/Axes, chart types, layouts, styling, dashboards |

---

## 📚 Table of Contents

- [Repository structure](#-repository-structure)
- [The 15 sessions](#-the-15-sessions)
- [Learning path](#-learning-path)
- [Getting started](#-getting-started)
- [How to use each session](#-how-to-use-each-session)
- [Projects you will build](#-projects-you-will-build)
- [Cheat sheets](#-cheat-sheets)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🗂️ Repository structure

```text
python-data-lab/
├── README.md
├── requirements.txt
├── LICENSE
│
├── 01-numpy/
│   ├── README.md                ← track overview
│   ├── cheatsheet.md            ← one-page NumPy reference
│   ├── session-01-numpy-foundations-and-ndarray/
│   │   ├── README.md            ← theory & concepts
│   │   ├── notebook.ipynb       ← runnable Build It + Experiment
│   │   ├── exercises.md         ← practice (no answers)
│   │   ├── exercises_solutions.ipynb
│   │   └── ai-learning.md
│   ├── session-02-reshaping-and-indexing/
│   ├── session-03-boolean-indexing-operations-broadcasting/
│   ├── session-04-math-functions-and-grayscale-project/
│   └── session-05-random-sampling-and-file-io/
│
├── 02-pandas/
│   ├── README.md
│   ├── cheatsheet.md            ← one-page Pandas reference
│   ├── session-01-series-and-dataframes/
│   ├── session-02-indexing-selecting-filtering/
│   ├── session-03-cleaning-data/
│   ├── session-04-groupby-merging/
│   └── session-05-time-series-and-io/
│
└── 03-matplotlib/
    ├── README.md
    ├── cheatsheet.md            ← one-page Matplotlib reference
    ├── session-01-figure-axes-basics/
    ├── session-02-plot-types/
    ├── session-03-subplots-layouts-annotations/
    ├── session-04-styling-and-colormaps/
    └── session-05-data-storytelling-dashboard/
```

Every session uses the **same 5-file layout**, so once you learn the rhythm you can move fast.

---

## 📖 The 15 sessions

### 🔢 NumPy Track

| # | Session | Core topics | Mini project |
|:-:|---|---|---|
| **01** | [Foundations & `ndarray`](./01-numpy/session-01-numpy-foundations-and-ndarray/README.md) | dtypes, `array`/`zeros`/`empty`/`arange`/`linspace`, `ndim`/`size`/`shape` | 🔍 Array inspector |
| **02** | [Reshaping & Indexing](./01-numpy/session-02-reshaping-and-indexing/README.md) | `reshape`/`ravel`/`flatten`, views vs. copies, slicing | ✂️ Matrix cropper |
| **03** | [Boolean, Ops & Broadcasting](./01-numpy/session-03-boolean-indexing-operations-broadcasting/README.md) | Masks, AND/OR, element-wise vs. `@`, broadcasting | 🎯 Grade filter & curve |
| **04** | [Vectorized Math & Grayscale](./01-numpy/session-04-math-functions-and-grayscale-project/README.md) | `sqrt`/`power`/`sin`/`log`, `np.emath`, images | 🖼️ Grayscale + threshold |
| **05** | [Random Sampling & File I/O](./01-numpy/session-05-random-sampling-and-file-io/README.md) | Distributions, seeds, `.npz`, `.csv` | 📡 Persist a dataset |

### 🐼 Pandas Track

| # | Session | Core topics | Mini project |
|:-:|---|---|---|
| **01** | [Series & DataFrames](./02-pandas/session-01-series-and-dataframes/README.md) | `Series`, `DataFrame`, inspection, column selection | 🎓 Student Gradebook |
| **02** | [Indexing, Selecting & Filtering](./02-pandas/session-02-indexing-selecting-filtering/README.md) | `loc`/`iloc`, Boolean masks, `query`, sorting | 🛒 Sales Explorer |
| **03** | [Cleaning Data](./02-pandas/session-03-cleaning-data/README.md) | Missing values, dtypes, duplicates, `.str` | 🧹 Messy CSV Cleanup |
| **04** | [GroupBy & Merging](./02-pandas/session-04-groupby-merging/README.md) | `groupby`/`agg`, `pivot_table`, `merge`, `concat` | 🧾 Orders + Customers |
| **05** | [Time Series & I/O](./02-pandas/session-05-time-series-and-io/README.md) | `to_datetime`, `resample`, `rolling`, CSV/Excel/Parquet | 📅 Daily Sales |

### 📊 Matplotlib Track

| # | Session | Core topics | Mini project |
|:-:|---|---|---|
| **01** | [Figure/Axes & First Plots](./03-matplotlib/session-01-figure-axes-basics/README.md) | Figure/Axes model, `plot`, labels, legends, `savefig` | 📈 Plot a Function |
| **02** | [Plot Types](./03-matplotlib/session-02-plot-types/README.md) | `scatter`, `bar`, `hist`, `boxplot`, `pie`, `errorbar` | 📊 Distribution & Comparison |
| **03** | [Subplots, Layouts & Annotations](./03-matplotlib/session-03-subplots-layouts-annotations/README.md) | Grids, `GridSpec`, `twinx`, `annotate` | 🧩 2×2 Dashboard |
| **04** | [Styling & Colormaps](./03-matplotlib/session-04-styling-and-colormaps/README.md) | Colors, `rcParams`, style sheets, colormaps, heatmaps | 🎨 Chart Makeover |
| **05** | [Data Storytelling & Dashboards](./03-matplotlib/session-05-data-storytelling-dashboard/README.md) | pandas + matplotlib, multi-panel figures, best practices | 📰 Sales Dashboard |

---

## 🧭 Learning path

Work through the tracks in order and tick the boxes as you go:

**🔢 NumPy**
- [ ] Session 1 — build the array intuition
- [ ] Session 2 — master views vs. copies and indexing
- [ ] Session 3 — predict broadcasting shapes
- [ ] Session 4 — convert a photo to grayscale
- [ ] Session 5 — simulate, save, and reload data

**🐼 Pandas**
- [ ] Session 1 — create and inspect DataFrames
- [ ] Session 2 — select and filter like a pro
- [ ] Session 3 — clean messy real-world data
- [ ] Session 4 — group, aggregate, and join tables
- [ ] Session 5 — analyse a time series and save it

**📊 Matplotlib**
- [ ] Session 1 — understand Figure and Axes
- [ ] Session 2 — pick the right chart type
- [ ] Session 3 — lay out subplots and annotate
- [ ] Session 4 — style with colors and colormaps
- [ ] Session 5 — build a full dashboard

---

## 🚀 Getting started

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/python-data-lab.git
cd python-data-lab

# 2. (Recommended) Create and activate a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # macOS / Linux

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook
```

Then open any session's `notebook.ipynb` and run the cells top to bottom. All datasets are generated inline, so nothing else is required.

---

## 🧑‍🎓 How to use each session

1. **Read** `README.md` — build the mental model (intuition → formal → code).
2. **Run** `notebook.ipynb` — execute every cell; predict outputs before running.
3. **Attempt** `exercises.md` — try all 5 before looking anywhere else.
4. **Compare** `exercises_solutions.ipynb` — check *why* the solution works.
5. **Go deeper** with `ai-learning.md` — code review, debugging, and AI challenges.
6. **Build** the mini project, then move on.

---

## 🏗️ Projects you will build

| Track | Project | What it proves |
|:-:|---|---|
| 🔢 | Array Inspector | Shape/size/dtype introspection |
| 🔢 | Matrix Cropper | Slicing and memory management |
| 🔢 | Grade Filter & Curve | Filtering, transforming, normalizing |
| 🔢 | Grayscale + Threshold | Image processing with pure NumPy |
| 🔢 | Sensor Data Round-Trip | Simulate, save, reload, verify |
| 🐼 | Student Gradebook | DataFrame creation and summarization |
| 🐼 | Sales Explorer | `loc`/`iloc`, filtering, sorting |
| 🐼 | Messy CSV Cleanup | Missing values, dtypes, duplicates |
| 🐼 | Orders + Customers | GroupBy, pivot, merge |
| 🐼 | Daily Sales Time Series | Datetime, resample, rolling, I/O |
| 📊 | Plot a Function | Figure/Axes anatomy |
| 📊 | Distribution & Comparison | Choosing chart types |
| 📊 | 2×2 Dashboard | Layouts and annotations |
| 📊 | Chart Makeover | Styling and colormaps |
| 📊 | Sales Dashboard | End-to-end data storytelling |

---

## 📄 Cheat sheets

One-page, print-friendly references — one per track:

| Track | Cheat sheet |
|---|---|
| 🔢 NumPy | [NumPy Cheat Sheet](./01-numpy/cheatsheet.md) |
| 🐼 Pandas | [Pandas Cheat Sheet](./02-pandas/cheatsheet.md) |
| 📊 Matplotlib | [Matplotlib Cheat Sheet](./03-matplotlib/cheatsheet.md) |

---

## 🤝 Contributing

Found a typo, a clearer explanation, or a bug? Contributions are welcome.

1. Fork the repository
2. Create a branch: `git checkout -b fix/pandas-session-03-typo`
3. Commit your change with a clear message
4. Open a Pull Request

Please keep the 5-file session structure and the beginner-friendly tone.

---

## 📜 License

Released under the [MIT License](./LICENSE). Use it, teach with it, remix it.

---

<div align="center">

**If this lab helped you, consider giving it a ⭐ — it helps others find it!**

Made with 🐍 · 🐼 · 📊

</div>
