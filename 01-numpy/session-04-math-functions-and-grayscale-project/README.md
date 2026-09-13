# 📘 Session 4 — Vectorized Math & the Grayscale Project

<p align="center">
  <a href="../../README.md">🏠 Lab Home</a> ·
  <a href="../README.md">🔢 NumPy Track</a> ·
  <a href="./notebook.ipynb">📓 Notebook</a> ·
  <a href="./exercises.md">🧪 Exercises</a> ·
  <a href="./exercises_solutions.ipynb">✅ Solutions</a> ·
  <a href="./ai-learning.md">🤖 AI Learning</a> ·
  <a href="../session-03-boolean-indexing-operations-broadcasting/README.md">⬅️ Session 3</a> ·
  <a href="../session-05-random-sampling-and-file-io/README.md">Session 5 ➡️</a>
</p>

> **Goal:** apply math functions across whole arrays, understand `nan` vs. complex results, and build a real color→grayscale image pipeline.

📁 **In this folder:**
- `README.md` — this file (theory & concepts)
- `notebook.ipynb` — runnable code: Build It + Experiment sections (auto-generates a placeholder image if you don't provide `profile.png`)
- `exercises.md` — practice exercises (no answers)
- `exercises_solutions.ipynb` — worked solutions to the exercises + mini project
- `ai-learning.md` — how to learn this session with an AI assistant

> 💡 **Tip:** drop your own photo into this folder and name it `profile.png` to see the real grayscale conversion on your own image. Otherwise the notebooks generate a synthetic placeholder automatically.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Apply mathematical functions (`np.sqrt`, `np.power`, `np.sin`, `np.log`) to entire arrays at once, without writing a `for` loop.
- Understand what happens when a real-valued math function receives an input with no real answer (e.g. `sqrt` of a negative number), and how `np.emath` solves this in the complex domain.
- Load an image into Python as a NumPy array using Pillow, and understand how a color image maps onto a 3D array of shape `(height, width, 3)`.
- Convert a color image to grayscale using a standard luminance formula, implemented as a NumPy matrix operation.
- Save the resulting grayscale array back out as an image file.

---

## 🧠 Concept Explanation

### 4.1 Vectorized math functions

**Intuition.** In plain Python, if you wanted the square root of every number in a list, you'd import `math`, write a loop, and call `math.sqrt()` on each element. NumPy instead gives you functions that operate on the **whole array in one call** — no loop needed, and it runs at C speed. `np.prod` is a great first example: instead of writing a loop to multiply all elements together, one function call reduces the entire array to a single number.

Combine `np.linspace` (from Session 1) with `np.sin` to compute the sine of 100 evenly-spaced angles in one shot — NumPy computes the full curve instantly.

### 4.2 When real math breaks down: `np.emath`

Some functions have no answer within the real numbers for certain inputs. Calling `np.sqrt(-4)` does **not** throw an exception — it returns `nan` ("**N**ot **a** **N**umber"), because square roots of negative numbers have no solution among real numbers.

**The question:** can we solve this in the **complex number** domain instead? Yes — NumPy provides a dedicated module for exactly this purpose: **`np.emath`** (NumPy's "complex-aware math" module). It re-implements many of the same function names (`sqrt`, `log`, etc.), but instead of returning `nan` for inputs with no real solution, it computes the answer as a **complex number**.

| Function | Behavior on negative input |
|---|---|
| `np.sqrt(-4)` | Returns `nan` |
| `np.emath.sqrt(-4)` | Returns a valid complex number |
| `np.log(-2)` | Returns `nan` |
| `np.emath.log(-2)` | Returns a valid complex number |

**Important nuance:** for **positive** inputs, `np.emath.sqrt` and `np.sqrt` return identical (real-valued) results. The two only diverge when the input would otherwise be undefined in the reals.

### 4.3 Real-world application: converting a color photo to grayscale

This section shows how the ideas from every previous lesson (arrays, shape, slicing, matrix multiplication) combine to solve a genuine image-processing task.

**Setup.** To read and write actual image files, you need the **Pillow** library alongside NumPy: `pip install pillow`.

**Step 1 — Load the image as a NumPy array.** Pillow's `Image.open()` gives you an `Image` object; `np.asarray()` converts it into a NumPy `ndarray`.

**Step 2 — Understand the shape.** For a 460×460 pixel photo, `img_data.shape` returns `(460, 460, 3)`:
- The first `460` is the number of pixel **rows** (image height).
- The second `460` is the number of pixel **columns** (image width).
- The `3` is the number of **color channels**: Red, Green, and Blue (RGB).

A color image is modeled as **three stacked 460×460 grids** — one for how "red" each pixel is, one for "green," one for "blue."

**Step 3 — Access an individual color channel.** Using the slicing skills from Session 2: `img_data[:, :, 0]` is the Red channel, `[:, :, 1]` is Green, `[:, :, 2]` is Blue. Each channel value is an integer from **0 to 255**.

**Step 4 — Apply the grayscale conversion formula.** The standard, perception-based formula is:

```
gray = 0.2126 * R + 0.7152 * G + 0.0722 * B
```

Framed as a matrix operation, the RGB image (shape `(H, W, 3)`) is multiplied by a weight vector `[0.2126, 0.7152, 0.0722]` (shape `(3,)`) using **matrix multiplication** (`@`), producing a single `(H, W)` grayscale matrix — no manual pixel loop required.

**Step 5 — Convert to a valid image data type.** The matrix multiplication result contains **floating-point** numbers. To save it as an image, cast it to **8-bit unsigned integers** (`uint8`) with `.astype(np.uint8)`.

**Step 6 — Save the result.** Convert the NumPy array back into a Pillow `Image` object with `Image.fromarray(...)`, then `.save(...)`.

The end-to-end pipeline: **image file → Pillow `Image` → NumPy array `(H, W, 3)` → matrix multiplication with luminance weights `(H, W)` → cast to `uint8` → Pillow `Image` → saved file.**

---

## ⚙️ Why This Matters

- **Signal & image processing:** vectorized math functions are the backbone of transforming raw signal/pixel data without expensive Python-level loops.
- **Complex-domain math:** fields like electrical engineering (AC circuit analysis), signal processing (Fourier transforms), and quantum mechanics regularly require complex-number math — `np.emath` bridges real-valued NumPy functions into that domain when needed.
- **Computer vision preprocessing:** virtually every computer vision pipeline (including deep learning ones) begins with representing images as NumPy/tensor arrays and applying vectorized transformations like grayscale conversion, normalization, or channel manipulation.
- **Performance:** applying a formula across a 460×460×3 image (over 600,000 numbers) via matrix multiplication is dramatically faster than a triple-nested Python loop over rows, columns, and channels.

---

## 🔍 Key Insights

- NumPy math functions (`np.sqrt`, `np.power`, `np.sin`, `np.log`, etc.) operate element-wise across an entire array in a single call — no explicit loop needed.
- When a real-valued function has no real answer for a given input (like `sqrt` of a negative number), NumPy returns `nan` rather than crashing.
- `np.emath` re-implements many of these same functions to compute answers in the **complex domain** instead of returning `nan` — and for valid (positive) real inputs, it returns identical results to the standard functions.
- A color image is naturally modeled as a **3D NumPy array**: `(height, width, channels)`, where channels are typically Red, Green, Blue, each an integer 0–255.
- The grayscale conversion is a real-world instance of **matrix multiplication**: an `(H, W, 3)` array times a `(3,)` weight vector produces an `(H, W)` result — this is exactly the linear algebra concept from the previous lesson applied to a concrete problem.
- A common mistake: forgetting to cast the final grayscale array to `np.uint8` before saving — image formats expect integer pixel values in the 0–255 range, not raw floating-point numbers.

---

## 📦 Summary

NumPy's vectorized math functions let you apply operations like square roots, powers, sines, and logarithms across entire arrays without loops, and the `np.emath` module extends several of these functions into the complex domain for inputs that have no real solution. Applying these ideas practically, a color photo naturally becomes a 3D NumPy array of shape `(height, width, 3)`, and converting it to grayscale is simply a matrix multiplication between the image array and a standard luminance weight vector — a compact, elegant demonstration of how array shapes, slicing, and matrix multiplication combine to solve a genuine real-world image-processing task.

---

## ⚡ Quick Reference

| Task | Idiomatic call |
|---|---|
| Element-wise sqrt | `np.sqrt(a)` |
| Power | `np.power(a, 3)` |
| Sine over a range | `np.sin(np.linspace(-np.pi/2, np.pi/2, 100))` |
| Product of all elements | `np.prod(a)` |
| Log (real → `nan` on negatives) | `np.log(a)` |
| Complex-aware math | `np.emath.sqrt(a)`, `np.emath.log(a)` |
| Image → array | `np.asarray(Image.open("profile.png"))` |
| Grayscale via matmul | `img @ np.array([0.2126, 0.7152, 0.0722])` |
| Array → image | `Image.fromarray(gray.astype(np.uint8)).save("out.png")` |

---

<p align="center">
  <a href="./notebook.ipynb">📓 Open the notebook</a> ·
  <a href="./exercises.md">🧪 Attempt the exercises</a> ·
  <a href="./ai-learning.md">🤖 Learn with AI</a> ·
  <a href="../session-05-random-sampling-and-file-io/README.md">Session 5 ➡️</a>
</p>
