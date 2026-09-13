# 📘 Session 3 — Boolean Indexing, Operations & Broadcasting

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="../README.md">🔢 NumPy Track</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../session-02-reshaping-and-indexing/README.md">⬅️ Session 2</a> ·
  <a href="../session-04-math-functions-and-grayscale-project/README.md">Session 4 ➡️</a>
</p>

> **Goal:** filter with Boolean masks, combine conditions safely, separate element-wise math from matrix multiplication, and apply the broadcasting rule.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Filter array elements using conditional expressions (**Boolean indexing**).
- Combine multiple conditions correctly using NumPy's `&` (and) and `|` (or) operators.
- Perform element-wise arithmetic between arrays (`+`, `*`, `**`) and know their function equivalents (`np.add`, `np.multiply`, `np.power`).
- Perform true matrix multiplication using `@` or `np.matmul`, and distinguish it from element-wise multiplication.
- Explain and apply NumPy's **broadcasting** rule to operate on arrays of different (but compatible) shapes without writing loops.

---

## 🧠 Concept Explanation

### 3.1 Boolean Indexing

**Intuition.** Suppose you have an array and you want only the elements smaller than 5. Instead of writing a loop, you can literally write the condition `arr < 5` inside the square brackets, and NumPy hands back only the matching elements.

**Formal mechanism.** Consider `a[a < 5]`. Here's what actually happens in two steps:

1. `a < 5` is evaluated **first**, on its own. It produces a **new NumPy array with the exact same shape as `a`**, but filled with `True`/`False` — `True` wherever the corresponding element of `a` satisfies the condition, `False` otherwise. This is often called a **Boolean mask**.
2. That mask is then used *as* the index: `a[mask]`. NumPy walks through the mask and keeps only the elements of `a` at positions where the mask is `True`, discarding the rest. The result is always a flat 1D array of the matching values.

You can also store the condition first, for readability:

```python
condition = a >= 3
filtered = a[condition]
```

**Combining conditions with `&` and `|`.** In plain Python you'd use the keywords `and`/`or` to combine two boolean values. But here we're not combining two single booleans — we're combining two **entire arrays**, element by element. NumPy defines two special operators for this purpose:

- **`&`** — element-wise logical AND
- **`|`** — element-wise logical OR

```python
a[(a > 2) & (a < 11)]     # elements strictly between 2 and 11
a[(a < 2) | (a % 3 == 0)] # elements < 2, OR divisible by 3
```

⚠️ **Critical rule:** whenever you combine more than one condition, you **must wrap each individual condition in parentheses**. Without parentheses, Python's operator precedence rules will try to evaluate `&`/`|` before the comparison operators, causing an error or wrong result. Always write `(condition_1) & (condition_2)`, never `condition_1 & condition_2` bare.

### 3.2 Element-wise operations and Matrix multiplication

**Element-wise arithmetic.** Given two vectors of the same shape, NumPy overloads the standard Python operators so they apply **element by element** (this is possible because `ndarray` implements Python's "dunder" methods, e.g. `__add__`, the same mechanism regular Python objects use to support `+`). Each operator has an exactly equivalent function form:

| Operator | Equivalent function |
|---|---|
| `a + b` | `np.add(a, b)` |
| `a * b` | `np.multiply(a, b)` |
| `a ** b` | `np.power(a, b)` |

Both forms (operator or function) produce identical output — the operator form is just syntactic sugar.

**Matrix multiplication is different.** If `a` and `b` are 2D matrices, `a * b` still performs **element-wise** multiplication (not true matrix multiplication!). To perform actual matrix multiplication (row × column dot products), you need a dedicated operator/function: `x @ y` or `np.matmul(x, y)`.

**Summary table:**

| Goal | How |
|---|---|
| Element-wise add | `a + b` or `np.add(a, b)` |
| Element-wise multiply | `a * b` or `np.multiply(a, b)` |
| Element-wise power | `a ** b` or `np.power(a, b)` |
| True matrix multiplication | `a @ b` or `np.matmul(a, b)` |

### 3.3 Broadcasting

**Intuition.** Broadcasting is what lets you write `array * 2` and have NumPy "stretch" the scalar `2` across every element of `array` — without you writing any loop, and without NumPy actually wasting memory duplicating that `2` many times. NumPy performs this "virtual stretching" internally, in compiled C code, in parallel — which is why broadcasting can dramatically increase performance versus a manual Python loop.

**Formal rule.** Broadcasting allows NumPy to perform element-wise operations between arrays of **different (but compatible) shapes**, by conceptually replicating the smaller array along the mismatched dimension(s) until the shapes match — then performing the operation as normal.

> For each pair of aligned dimensions, they must either (a) be equal, or (b) one of them must be 1 (or absent) — in which case that dimension gets "copied" to match the other.

Some worked cases:

1. **Array `(M, N)` with scalar / array of shape `(1, N)`:** the `(1, N)` array (or scalar) gets copied `M` times to become `(M, N)`, then the operation is applied elementwise, result is `(M, N)`.
2. **Array `(M, N)` with array `(M, 1)`:** the `(M, 1)` array gets copied `N` times (along the column axis) to become `(M, N)`, result is `(M, N)`.
3. **Array `(M, 1)` with array `(1, N)`:** *both* arrays get stretched — result is `(M, N)`.

If the shapes are **not** compatible under this rule (neither dimension is 1, and they aren't equal), NumPy raises an error rather than silently guessing what you meant.

**Real-world application: normalization.** A very common use of broadcasting is standardizing (normalizing) data — shifting the mean to 0 and scaling the variance to 1 — without writing a single loop: `(a - a.mean()) / a.std()`. Here, `a.mean()` and `a.std()` are single scalars, and broadcasting automatically applies the subtraction and division to every element of `a`.

---

## ⚙️ Why This Matters

- **Data cleaning & filtering:** Boolean indexing is the standard way to filter datasets — e.g., "give me all sensor readings above a threshold," or "give me all prices below the median," without manual loops.
- **Feature engineering / normalization (ML):** standardizing features (zero mean, unit variance) is a near-universal preprocessing step before training machine learning models, and it relies entirely on broadcasting.
- **Linear algebra & simulation:** matrix multiplication (`@`/`np.matmul`) underlies neural network layers, coordinate transformations in graphics/robotics, and systems of linear equations in engineering.
- **Performance:** broadcasting moves loop-like work into NumPy's compiled C backend, which can run operations in parallel — this is a major reason vectorized NumPy code massively outperforms hand-written Python loops on large datasets.

---

## 🔍 Key Insights

- Boolean indexing works in **two steps**: first the condition builds a same-shaped `True`/`False` mask, then that mask is used to filter the array. The output is always flattened to 1D.
- Always **parenthesize** individual sub-conditions when combining them with `&` or `|` — this is not optional style, it's required to avoid operator-precedence bugs.
- `a * b` on 2D arrays is **element-wise**, not matrix multiplication — a very common beginner confusion. Use `@` or `np.matmul` for true matrix multiplication.
- Every element-wise operator (`+`, `*`, `**`) has an exact function equivalent (`np.add`, `np.multiply`, `np.power`) — useful when you need to pass the operation itself as an argument to another function.
- Broadcasting's general rule: dimensions are compatible if they're equal, or if one of them is `1` (that side gets virtually "copied" to match). If neither holds, NumPy raises an error instead of guessing.
- Broadcasting is not just convenient syntax — it moves the "loop" into NumPy's parallelized C backend, which is why it's dramatically faster than an equivalent Python `for` loop.

---

## 📦 Summary

Boolean indexing lets you filter arrays with plain comparison expressions, as long as multiple conditions are combined with `&`/`|` and wrapped in parentheses. Element-wise arithmetic operators (`+`, `*`, `**`) apply per-element and have exact function equivalents, while true matrix multiplication requires the dedicated `@`/`np.matmul` operator. Broadcasting is the mechanism that lets NumPy operate on arrays of different but compatible shapes by conceptually "stretching" the smaller array — enabling powerful one-line operations like normalization without ever writing an explicit loop.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Filter with a condition | `a[a > 5]` |
| Combine conditions (AND) | `a[(a > 2) & (a < 8)]` |
| Combine conditions (OR) | `a[(a < 2) \| (a > 8)]` |
| Element-wise add / mul / pow | `a + b`, `a * b`, `a ** b` |
| Function equivalents | `np.add`, `np.multiply`, `np.power` |
| True matrix multiplication | `a @ b` or `np.matmul(a, b)` |
| Normalize (z-score) | `(a - a.mean()) / a.std()` |
| Pick by condition | `np.where(cond, x, y)` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-04-math-functions-and-grayscale-project/README.md">Session 4 ➡️</a>
</p>
