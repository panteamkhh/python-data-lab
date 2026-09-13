# 📘 Session 1 — NumPy Foundations & the `ndarray`

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="../README.md">🔢 NumPy Track</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../session-02-reshaping-and-indexing/README.md">Session 2 ➡️</a>
</p>

> **Goal:** install NumPy, understand the `ndarray`, create 1D/2D/3D arrays, and read `ndim`, `size`, and `shape`.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Explain what NumPy is, why it exists, and why it is fast.
- Install NumPy correctly inside an isolated virtual environment using `pip`.
- Explain what an `ndarray` (N-Dimensional Array) is and how it differs from a Python `list`.
- Create 1D NumPy arrays using `np.array`, `np.zeros`, `np.empty`, `np.arange`, and `np.linspace`.
- Create and reason about 2D and 3D arrays (matrices and tensors).
- Read and interpret the three core array attributes: `ndim`, `size`, and `shape`.

---

## 🧠 Concept Explanation

### 1.1 What is NumPy?

**Intuition first.** Imagine you want to multiply every item in a list of 1 million numbers by 2. In plain Python, you'd write a `for` loop that touches each element one at a time. NumPy's whole purpose is to let you skip that loop and instead say "multiply the whole block of numbers by 2" in one instruction — and have that instruction run at near-C speed.

**Formal explanation.** NumPy (**Num**erical **Py**thon) is a library for **scientific computing** in Python. Its core data structure is the `ndarray`, and its power comes from **vectorized operations**: computations expressed over whole arrays instead of individual elements. NumPy supports:

- Vector, matrix, and general N-dimensional array computation (linear algebra).
- Fourier transforms.
- A large collection of mathematical functions.
- Random number generation from many probability distributions.

**Why it's fast.** NumPy's Python-facing API is written in Python, but the actual numeric engine underneath is written in **C**. So when you call a NumPy function, your Python code is just a thin wrapper that dispatches the heavy lifting to compiled C code. You get C-like speed while writing Python syntax.

**Why it's foundational.** NumPy is so central to the Python data ecosystem that many other libraries are built directly on top of it:
- **pandas** (DataFrames) is built on NumPy arrays.
- **SciPy** (scientific computing) is built on NumPy.
- Deep learning libraries like **PyTorch** borrow core ideas (and interoperate with) NumPy arrays.
- Visualization libraries (like Matplotlib) consume NumPy arrays heavily.

Even if you end up using pandas or SciPy instead of raw NumPy, understanding NumPy helps you understand *why* those libraries behave the way they do.

### 1.2 Installing NumPy

NumPy's own documentation lists two installation routes: **conda** or **pip**. This lesson uses `pip`, with a virtual environment (`venv`) as a best practice — this keeps the packages you install isolated from your system-wide Python and from other projects.

Step-by-step workflow (run in your terminal, inside your project folder):

```bash
# 1. Create an isolated virtual environment named "venv"
python3 -m venv venv

# 2. Activate the virtual environment
source venv/bin/activate      # macOS / Linux
# venv\Scripts\activate        # Windows

# 3. Make sure pip itself is up to date
pip install --upgrade pip

# 4. Install NumPy inside the isolated environment
pip install numpy

# 5. When you're done working, leave the environment
deactivate
```

Once activated, any package you install with `pip` goes into the hidden `venv` folder instead of polluting your global Python installation. You only repeat steps 1–4 the first time; afterward you simply `activate` and get to work.

To confirm NumPy installed correctly, write a tiny script:

```python
import numpy as np

print(np.__version__)
```

Running `python main.py` should print a version string (e.g. `2.6.x`), matching what `pip install numpy` reported.

> **Best practice note:** we write `import numpy as np` instead of `import numpy` everywhere. This way we call functions as `np.something(...)` instead of typing `numpy.something(...)` repeatedly. Virtually the entire NumPy ecosystem follows this `np` convention.

### 1.3 The `ndarray`: NumPy's core data structure

NumPy's key data structure is called **`ndarray`**, short for **N-Dimensional Array object**.

**How is it different from a Python `list`?**

| | Python `list` | NumPy `ndarray` |
|---|---|---|
| Element types | Can mix types (int, str, object...) | Must be **homogeneous** — all elements share the same dtype |
| Typical use | General-purpose container | Numerical / vector computation |
| Speed | Pure Python loop speed | C-speed vectorized operations |
| Memory usage | Higher (each element is a full Python object) | Lower (packed, fixed-size numeric blocks) |

Two concrete advantages follow from this:

1. **Speed** — because NumPy is written in C, operations like multiplication, division, and other linear algebra tasks execute far faster than an equivalent Python `for` loop.
2. **Memory** — because all elements are the same type (**homogeneous**), NumPy arrays occupy noticeably less memory than the equivalent Python list.

**What do NumPy arrays model in the real world?** Depending on how many dimensions (axes) they have, `ndarray`s can represent:

- **Vector** — a 1-dimensional array (1 axis).
- **Matrix** — a 2-dimensional array (2 axes), i.e., rows and columns.
- **Tensor** — a general N-dimensional array (N axes), the term used in math/ML when N > 2.

### 1.4 Creating 1D arrays (vectors)

The simplest way to build an `ndarray` is `np.array(...)`, passing in a regular Python list. Even though printing an array looks visually similar to a Python list, `type(a)` confirms it is an `ndarray`. You index it exactly like a Python list: `a[0]`, `a[1]`, etc.

Besides `np.array`, NumPy provides convenience constructors:

- **`np.zeros(n)`** — creates a 1D array of length `n` where every element is `0`.
- **`np.empty(n)`** — creates a 1D array of length `n`, but does **not** initialize the values. Whatever bytes happen to already sit at that memory address are shown — i.e., it looks like random garbage. Its advantage is pure speed: since NumPy skips the initialization step, `np.empty` is faster to create than `np.zeros`. You use it only when you plan to immediately fill in every element yourself, not when you need the array to start as meaningful data.
- **`np.arange(start, stop, step)`** — analogous to Python's built-in `range`, but with two crucial differences: (1) it directly returns an `ndarray`, not a lazy generator, and (2) the `stop` value is **exclusive** (not included), exactly like `range`.
- **`np.linspace(start, stop, num)`** — generates `num` evenly spaced numbers between `start` and `stop`, where **both endpoints are included** (inclusive on both sides). This is different from `arange`, where you specify a step size; with `linspace` you specify how many points you want, and NumPy figures out the spacing.

### 1.5 Creating N-dimensional arrays (matrices & tensors)

To go from 1D to 2D, you nest lists inside a list — a "list of lists." The outer list becomes the rows, and each inner list becomes the columns — so the result is a 2D array, i.e., a **matrix**.

To go one level further, to 3D, you nest one more layer — a "list of lists of lists." Reading the shape from the outside in: outer blocks → each block's rows → each row's columns.

### 1.6 Array attributes: `ndim`, `size`, `shape`

Every `ndarray` automatically stores metadata about its own structure, exposed as attributes:

- **`.ndim`** — the **number of dimensions** (axes) the array has. A vector has `ndim == 1`, a matrix has `ndim == 2`, and so on.
- **`.size`** — the **total number of elements** in the array (regardless of shape).
- **`.shape`** — a tuple stating **how many elements exist along each axis**. Reading it left to right walks from the outermost axis to the innermost axis.

A useful mental check: multiplying all the numbers in `.shape` together always equals `.size`.

---

## ⚙️ Why This Matters

- **Simulation & scientific computing:** vectors and matrices are the native language of physics, chemistry, astronomy, and signal processing simulations — all fields NumPy is used in.
- **Machine learning & deep learning:** every dataset, weight matrix, and batch of training data is represented as an N-dimensional array (a "tensor"). Frameworks like PyTorch build directly on these ideas.
- **Data science:** libraries like pandas and SciPy are themselves built on top of NumPy arrays, so understanding `ndarray` mechanics helps you understand *their* internal behavior too.
- **Engineering systems:** linear algebra operations that power control systems, robotics, and graphics pipelines are all expressed as array/matrix math.

---

## 🔍 Key Insights

- NumPy arrays must be **homogeneous** — one dtype for the whole array — which is exactly what makes them fast and memory-efficient compared to Python lists.
- `np.empty` is fast precisely *because* it skips initialization — never assume its contents are zero.
- `np.arange`'s stop value is **exclusive**; `np.linspace`'s stop value is **inclusive**. This is one of the most common sources of off-by-one bugs for beginners.
- `ndim`, `size`, and `shape` are read-only descriptive attributes computed automatically — you never set them manually when creating an array; they are a *consequence* of the data you provided.
- A common misunderstanding: an `ndarray` that "looks like" a nested list when printed is *not* a Python list — `type()` will always confirm it's `numpy.ndarray`.
- Virtual environments are not NumPy-specific — they are a general Python best practice to isolate project dependencies.

---

## 📦 Summary

NumPy is a C-powered numerical computing library that underlies most of the Python scientific ecosystem, including pandas, SciPy, and deep learning frameworks. Its core object, the `ndarray`, is a homogeneous, memory-efficient, fast alternative to Python lists, and it can represent vectors (1D), matrices (2D), or tensors (N-D). You install it with `pip install numpy`, ideally inside a `venv`. Arrays are built with `np.array`, `np.zeros`, `np.empty`, `np.arange`, or `np.linspace`, and every array carries self-describing metadata via `.ndim`, `.size`, and `.shape`.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Alias NumPy | `import numpy as np` |
| From a Python list | `np.array([1, 2, 3])` |
| Zeros / ones | `np.zeros((2, 3))`, `np.ones((2, 3))` |
| Uninitialized (fast) | `np.empty(5)` |
| Range with a step | `np.arange(0, 10, 2)` |
| Evenly spaced count | `np.linspace(0, 1, 5)` |
| Dimensions / size / shape | `a.ndim`, `a.size`, `a.shape` |
| Data type | `a.dtype` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-02-reshaping-and-indexing/README.md">Session 2 ➡️</a>
</p>
