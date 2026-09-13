# 📘 Session 2 — Reshaping, Views, and Indexing

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="../README.md">🔢 NumPy Track</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../session-01-numpy-foundations-and-ndarray/README.md">⬅️ Session 1</a> ·
  <a href="../session-03-boolean-indexing-operations-broadcasting/README.md">Session 3 ➡️</a>
</p>

> **Goal:** reshape arrays, flatten them safely, tell views from copies, and index 1D/2D arrays with the comma syntax.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Change the shape (dimensions) of an existing array with `.reshape()`.
- Flatten any N-D array back down to 1D using `.reshape(-1)`, `.ravel()`, or `.flatten()`, and explain the difference between them.
- Distinguish between a **view** and a **copy** of an array, and know which operations return which.
- Index and slice 1D arrays exactly the way you would a Python list.
- Index and slice 2D arrays using comma-separated axis selectors (`array[rows, columns]`).

---

## 🧠 Concept Explanation

### 2.1 Reshaping arrays

**Intuition.** Reshaping doesn't change your data — it only changes how that data is *organized* into rows, columns, or higher dimensions. Think of it as pouring the same amount of water into a differently-shaped container: the volume (element count) stays the same.

**Formal rule.** `array.reshape(dim1, dim2, ...)` reorganizes the array's elements into the new shape. The one hard requirement: **the product of the new dimensions must equal the total number of elements in the original array.** If your original array has 12 elements, you can reshape it into `(3, 4)`, `(2, 6)`, `(12,)`, `(2, 2, 3)`, etc. — but never into a shape whose product isn't 12.

**Flattening back to 1D.** There are three common tools:

1. **`array.reshape(-1)`** — passing `-1` tells NumPy "figure out the single remaining dimension for me" — in practice this flattens the array to 1D.
2. **`array.ravel()`** — functionally equivalent to `reshape(-1)`.
3. **`array.flatten()`** — looks identical in output, but behaves differently *behind the scenes* (see next section).

### 2.2 View vs. Copy — the crucial hidden difference

This is one of the most important — and most overlooked — concepts in NumPy.

- **`reshape()`** and **`ravel()`** return a **view** of the original array whenever possible. A view does **not** duplicate the underlying data in memory; it's a new "window" onto the *same* memory. This makes it fast. The consequence: if you mutate elements through the view, you may be mutating the original array's data too (since they share memory).
- **`flatten()`** always returns a **copy** — technically a **deep copy**. It first creates an independent duplicate of the array's data in a new location in memory, and *then* flattens that duplicate. Because it has to duplicate data, it is slightly slower than `ravel()`/`reshape()`, but it is safer: changes to the flattened copy will **never** affect the original array.

| Method | Returns | Speed | Shares memory with original? |
|---|---|---|---|
| `reshape(-1)` | View (when possible) | Fast | Yes (usually) |
| `ravel()` | View (when possible) | Fast | Yes (usually) |
| `flatten()` | Copy (always) | Slightly slower | No — fully independent |

**Rule of thumb:** if you need to safely modify a flattened array without touching the original, use `flatten()`. If you just want to *read* a flattened view (or don't care about side effects), `reshape(-1)`/`ravel()` are faster.

### 2.3 Indexing 1D arrays

If your NumPy array is 1D, indexing works **exactly like Python list indexing** — including negative indices and slice syntax (`a[2:]`, `a[-3:-1]`, `a[2:4]`, etc.). Recall Python's slicing convention: `start:stop` means "from `start`, up to but not including `stop`." A bare `:` on one side means "to the beginning" or "to the end." NumPy 1D arrays honor these rules identically to Python lists.

### 2.4 Indexing 2D arrays: the comma syntax

Here's where NumPy indexing diverges from plain Python. A 2D NumPy array is **not** a "list of lists" from NumPy's point of view — it's a genuine 2D object. So instead of chaining two separate square-bracket lookups like you would with a nested Python list (`my_list[0][1]`), NumPy lets you separate axis selectors with a **comma inside one set of brackets**: `array[rows, columns]`.

**How to read `b[X, Y]`:**
- The part **before** the comma selects which **row(s)** (axis 0).
- The part **after** the comma selects which **column(s)** (axis 1).
- Each side can be a single index, a slice (`start:stop`), or a bare `:` meaning "take everything along this axis."

This comma-based pattern generalizes: a 3D array is indexed as `arr[axis0_selector, axis1_selector, axis2_selector]`, and so on for higher dimensions.

---

## ⚙️ Why This Matters

- **Data preprocessing (ML/Data Science):** raw data often needs reshaping — e.g., flattening a 28×28 image into a 784-length vector before feeding it to a machine learning model, or reshaping a batch of vectors back into images.
- **Memory efficiency:** knowing that `reshape`/`ravel` avoid copying data (unlike `flatten`) lets you write code that avoids unnecessary memory duplication in large-scale numerical pipelines.
- **Selecting sub-regions of data:** 2D indexing/slicing is exactly how you would extract a single feature column, a single sample row, or a rectangular sub-region from a dataset or image matrix.
- **Signal & image processing:** slicing along specific axes is how you isolate individual channels (e.g., a single color channel of an image, covered in a later project lesson).

---

## 🔍 Key Insights

- **Reshape never changes the data itself — only its organization.** The total element count is always conserved.
- **`reshape(-1)` and `ravel()` return views** (fast, memory-sharing); **`flatten()` returns a deep copy** (safe, independent, slightly slower).
- A common misunderstanding: assuming `flatten()` and `ravel()` behave identically just because their *output* looks the same. The difference only shows up when you *mutate* the result.
- **NumPy 2D arrays are not "lists of lists."** The idiomatic, more powerful NumPy way to index is the comma syntax: `arr[0, 1]`.
- The comma inside the brackets separates **axis selectors**: everything before the first comma applies to axis 0 (rows), everything after applies to axis 1 (columns), and so on for higher dimensions.
- A bare `:` means "take everything along this axis" — extremely useful for isolating a single row or column while keeping all elements along the other axis.

---

## 📦 Summary

Reshaping lets you reorganize an array's elements into new dimensions as long as the total element count stays constant, and `reshape(-1)`, `ravel()`, and `flatten()` all flatten arrays back to 1D — but only `flatten()` guarantees an independent deep copy, while the other two return fast memory-sharing views. Indexing 1D NumPy arrays works exactly like Python lists, but 2D (and higher) arrays require the comma syntax `arr[rows, cols]`, where each side can be a single index, a slice, or a bare `:` meaning "everything." Mastering this comma-based indexing is essential for extracting rows, columns, and sub-regions from real datasets and images.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Reshape | `a.reshape(3, 4)` |
| Auto-calculate one axis | `a.reshape(-1)` |
| Flatten → **view** | `a.ravel()` |
| Flatten → **copy** | `a.flatten()` |
| 1D slice | `a[1:4]` |
| Single element (2D) | `a[1, 2]` |
| Entire row / column | `a[0, :]` / `a[:, 0]` |
| Rectangular sub-block | `a[1:4, 2:5]` |
| Last rows / columns | `a[-2:, -2:]` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-03-boolean-indexing-operations-broadcasting/README.md">Session 3 ➡️</a>
</p>
