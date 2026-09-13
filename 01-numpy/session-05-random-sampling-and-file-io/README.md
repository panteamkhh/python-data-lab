# 📘 Session 5 — Random Sampling & File I/O

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="../README.md">🔢 NumPy Track</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../session-04-math-functions-and-grayscale-project/README.md">⬅️ Session 4</a> ·
  <a href="../cheatsheet.md">📄 Cheat Sheet</a>
</p>

> **Goal:** sample from probability distributions, make randomness reproducible, and persist arrays to `.npz` and `.csv` while understanding the dtype trade-offs.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Generate random samples from named probability distributions (Normal, Uniform, and others) using `np.random`.
- Set a random seed for reproducible results.
- Explain why sample statistics (mean, std) converge toward the true distribution parameters as sample size grows (an intuitive brush with the Law of Large Numbers).
- Save one or more NumPy arrays to a binary `.npz` file and reload them with `np.load`.
- Save a NumPy array to a human-readable `.csv` file with `np.savetxt` and reload it with `np.loadtxt`.
- Understand the data-type caveat when round-tripping arrays through text files.

---

## 🧠 Concept Explanation

### 5.1 Generating random numbers from a distribution

**Intuition.** Suppose you want to simulate the heights of people in a population. Real-world heights aren't uniformly random — they cluster around an average, with fewer people far above or below it. This clustering pattern is captured by the **Normal (Gaussian) distribution**, defined by two parameters: a **mean** (center) and a **variance/standard deviation** (spread). NumPy lets you generate as many simulated samples from this distribution as you like, in one call: `np.random.normal(mean, std, size=...)`.

**Convergence toward the true parameters.** The generated numbers are a **sample** drawn from the distribution — not the distribution itself. Because it's a small sample, its own mean and standard deviation will only be *approximately* equal to the true parameters — not exact. As you generate progressively more samples — say 1,000, then 1,000,000 — the sample's mean and standard deviation get closer and closer to the true distribution parameters. This is a hands-on illustration of a core statistical idea: **larger samples converge more tightly to the true underlying distribution.**

**The Uniform distribution.** Not all randomness should cluster around a center — sometimes every value in a range is equally likely: `np.random.uniform(low, high, size=...)`.

**Other distributions.** NumPy's `np.random` module supports many more named distributions beyond Normal and Uniform, including: **Pareto**, **Beta**, **Exponential**, **Gamma**, **Laplace**, **Multinomial**, and more. Each models a different real-world randomness pattern — for example, the Pareto distribution is famously associated with the "80/20 rule," and is often used to simulate income distributions. The right distribution to sample from depends entirely on what real-world phenomenon you're trying to simulate.

**Reproducibility with a seed.** Random generation is, by default, different every time you run your program. If you need the *same* "random" numbers again later, you fix a seed with `np.random.seed(42)`. Calling this before generating random numbers guarantees the exact same sequence of "random" values every time the script runs, as long as the seed value stays the same.

**Why this matters for deep learning.** Frameworks like TensorFlow rely heavily on NumPy-style random number generation behind the scenes to randomly initialize the weights of a neural network before training begins.

### 5.2 Saving and loading arrays: binary format (`.npz`)

NumPy provides its own dedicated **binary** file format for persisting arrays to disk.

- **`np.savez`** — the "z" stands for "zipped." You give it a filename ending in `.npz`, then any number of arrays as **keyword arguments** — each keyword becomes the label ("key") you'll use to retrieve that specific array later. The resulting `.npz` file is *not* human-readable — you need NumPy itself to make sense of its contents.
- **`np.load`** — returns an object that behaves like a dictionary of the arrays you saved; you retrieve each one using the same keyword you used when saving it with `savez`.

### 5.3 Saving and loading arrays: human-readable format (`.csv`)

Sometimes a binary `.npz` file isn't convenient — e.g., if you want to open the data in Excel, or share it with a program that doesn't understand NumPy's binary format.

**Building a 2D array to save.** `x[:, np.newaxis]` reshapes a 1D array into a column vector (adding a new axis, turning shape `(10,)` into `(10, 1)`). `np.hstack` then glues two such column vectors together **horizontally**, producing a two-column matrix — exactly the layout a CSV needs.

**Saving as CSV with `np.savetxt`:** `np.savetxt("xy.csv", array_2d, header="x,y", delimiter=",")` produces a genuinely human-readable text file you can open in a text editor, Excel, or any spreadsheet program.

**Loading it back with `np.loadtxt`:** reads the CSV back into a single 2D array — note this is a *different* loading function than `np.load`, used specifically for **text** files.

**⚠️ Important caveat — data types don't round-trip perfectly through text.** When NumPy writes numbers to a plain text file, it doesn't preserve their original data type — it converts everything into **64-bit floating-point** numbers, because a text file has no built-in concept of "this number is an integer." So after `np.loadtxt`, even originally-integer data comes back as floats (e.g., `3` becomes `3.0`). If you need the values back as integers, you must explicitly re-cast, e.g. with `.astype(int)`.

| Format | Human-readable? | Preserves original dtype? | Function pair |
|---|---|---|---|
| `.npz` (binary) | No | Yes | `np.savez` / `np.load` |
| `.csv` (text) | Yes | No — always becomes `float64` | `np.savetxt` / `np.loadtxt` |

---

## ⚙️ Why This Matters

- **Simulation & Monte Carlo methods:** research and engineering fields routinely need to generate thousands or millions of random samples from a specific distribution to model uncertainty or run simulations.
- **Machine learning weight initialization:** deep learning frameworks like TensorFlow and PyTorch use exactly this kind of random sampling under the hood to initialize neural network weights before training.
- **Reproducible research:** setting a random seed is essential in scientific computing and machine learning experiments so that results can be exactly reproduced and verified by others.
- **Data persistence & interoperability:** saving intermediate results to `.npz` avoids expensive recomputation, while saving to `.csv` allows data to be shared with non-NumPy tools like Excel or other programming languages.

---

## 🔍 Key Insights

- Random sampling functions like `np.random.normal(mean, std, size)` and `np.random.uniform(low, high, size)` generate a **sample** from a distribution — not the distribution itself, so its computed mean/std will only approximate the true parameters, more closely as sample size grows.
- Different real-world phenomena require different distributions (Normal for naturally-clustering data like heights, Uniform for equally-likely ranges like some exam scores, Pareto for skewed data like income) — NumPy's documentation lists the full catalog.
- `np.random.seed(...)` makes "random" results reproducible — essential for debugging and for reproducible scientific/ML experiments.
- `.npz` files are **binary** and preserve exact data (including dtype), but aren't human-readable; `.csv` files are human-readable but **always store numbers as floats**, even if the originals were integers.
- A common misunderstanding: assuming a CSV round-trip preserves the original data type. It never does automatically — you must explicitly re-cast with `.astype(...)` if you need integers back.
- `np.load` is the counterpart to `np.savez` (binary), while `np.loadtxt` is the counterpart to `np.savetxt` (text) — they are **not interchangeable** function pairs.

---

## 📦 Summary

NumPy's `np.random` module lets you generate samples from many named probability distributions (Normal, Uniform, Pareto, and more), and setting a seed with `np.random.seed()` makes those "random" results perfectly reproducible; as sample size grows, the sample's own statistics converge toward the true distribution parameters. For persistence, `.npz` files (via `np.savez`/`np.load`) store arrays in an efficient binary format that preserves exact data types but isn't human-readable, while `.csv` files (via `np.savetxt`/`np.loadtxt`) are human-readable and interoperable with tools like Excel, at the cost of always converting numbers to floating-point on reload. Together, these tools complete the practical NumPy workflow: generate or compute data, then reliably save and restore it.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Normal sample | `np.random.normal(mean, std, size=n)` |
| Uniform sample | `np.random.uniform(low, high, size=n)` |
| Reproducibility (legacy) | `np.random.seed(42)` |
| Reproducibility (modern) | `rng = np.random.default_rng(42)` |
| Save binary (multiple) | `np.savez("f.npz", x=x, y=y)` |
| Load binary | `np.load("f.npz")["x"]` |
| Save text | `np.savetxt("f.csv", a, delimiter=",")` |
| Load text | `np.loadtxt("f.csv", delimiter=",")` |
| Stack columns side by side | `np.hstack((x[:, None], y[:, None]))` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../cheatsheet.md">📄 Cheat Sheet</a> ·
  <a href="../../README.md">🏠 Back to Home</a>
</p>
