# 📄 NumPy Cheat Sheet

A one-page reference you can keep open while coding. Every snippet assumes:

```python
import numpy as np
```

---

## 1. Creating arrays

```python
np.array([1, 2, 3])                  # from a Python list
np.array([[1, 2], [3, 4]])           # 2D (matrix)
np.zeros((2, 3))                     # all zeros
np.ones((2, 3))                      # all ones
np.full((2, 3), 7)                   # all 7s
np.empty(5)                          # uninitialized (fast, garbage values)
np.arange(0, 10, 2)                  # [0 2 4 6 8]  (stop EXCLUSIVE)
np.linspace(0, 1, 5)                 # [0. 0.25 0.5 0.75 1.]  (both ends INCLUSIVE)
np.eye(3)                            # identity matrix
np.random.default_rng(42).normal(0, 1, size=(2, 3))
```

## 2. Inspecting an array

```python
a.ndim          # number of dimensions
a.shape         # tuple, e.g. (3, 4)
a.size          # total number of elements
a.dtype         # element data type, e.g. float64
a.itemsize      # bytes per element
a.nbytes        # total bytes
```

## 3. Reshaping / flattening

```python
a.reshape(3, 4)      # reorganize (element count must match)
a.reshape(-1)        # flatten to 1D (auto dimension) — VIEW
a.ravel()            # flatten — VIEW (usually)
a.flatten()          # flatten — always a COPY (safe to mutate)
a.T                  # transpose
```

> View vs. copy: a **view** shares memory with the original; mutating it mutates the source. A **copy** is independent.

## 4. Indexing & slicing

```python
a[0]                 # first element (1D) / first row (2D)
a[-1]                # last element
a[1:4]               # slice: indices 1, 2, 3 (stop exclusive)
a[::2]               # every other element

m[0, 1]              # row 0, column 1
m[:, 0]              # entire first column
m[0, :]              # entire first row
m[1:3, 2:4]          # rectangular sub-block
m[-2:, -2:]          # last 2 rows and last 2 columns
```

## 5. Boolean indexing

```python
a[a > 5]                       # keep elements > 5
a[(a > 2) & (a < 8)]           # AND  (parentheses REQUIRED)
a[(a < 2) | (a > 8)]           # OR
np.where(a > 5, 1, 0)          # conditional select
(a > 5).sum()                  # count of matches
```

## 6. Math & aggregation

```python
a + b, a - b, a * b, a / b     # element-wise
a ** 2                          # element-wise power
np.add(a, b), np.multiply(a, b) # function forms
a @ b, np.matmul(a, b)          # TRUE matrix multiplication

a.sum(), a.mean(), a.std()
a.min(), a.max(), a.argmax()
a.sum(axis=0)                   # sum down each column
a.sum(axis=1)                   # sum across each row

np.sqrt(a), np.exp(a), np.log(a)
np.abs(a), np.round(a, 2)
np.emath.sqrt(a)                # complex-aware (handles negatives)
```

## 7. Broadcasting (the rule)

Align shapes from the **right**; dimensions are compatible if they are **equal** or one of them is **1**.

```python
(3, 4) + (4,)     -> (3, 4)     # row broadcast
(3, 4) + (3, 1)   -> (3, 4)     # column broadcast
(3, 1) + (1, 4)   -> (3, 4)     # both stretch
(3, 4) + (3,)     -> ERROR      # 4 vs 3, neither is 1
```

```python
(a - a.mean()) / a.std()        # z-score normalization in one line
```

## 8. Random numbers

```python
np.random.seed(42)                                  # reproducible (legacy)
rng = np.random.default_rng(42)                     # modern Generator
rng.normal(loc=170, scale=5, size=(3, 4))
rng.uniform(low=0, high=1, size=100)
rng.integers(low=40, high=101, size=20)
```

> Larger samples → sample mean/std converge toward the true parameters (Law of Large Numbers).

## 9. Save & load

```python
np.savez("data.npz", x=x, y=y)          # binary, keeps dtype
loaded = np.load("data.npz")
loaded["x"]

np.savetxt("xy.csv", arr, header="x,y", delimiter=",")  # text
np.loadtxt("xy.csv", delimiter=",")                      # -> always float64
```

| Format | Human-readable | Keeps dtype |
|---|:-:|:-:|
| `.npz` (binary) | ❌ | ✅ |
| `.csv` (text) | ✅ | ❌ (float64) |

## 10. Images as arrays (Pillow)

```python
from PIL import Image
img = np.asarray(Image.open("photo.png"))     # (H, W, 3) uint8
img[:, :, 0]                                   # Red channel
gray = img @ np.array([0.2126, 0.7152, 0.0722])# luminance, (H, W)
Image.fromarray(gray.astype(np.uint8)).save("gray.png")
```

---

## ⚠️ Top beginner pitfalls

1. `np.arange` **excludes** the stop; `np.linspace` **includes** both ends.
2. `a * b` on matrices is element-wise — use `a @ b` for matrix multiplication.
3. Combining conditions requires parentheses: `(a > 2) & (a < 8)`.
4. `ravel()` / `reshape(-1)` return **views**; `flatten()` returns a **copy**.
5. `np.empty` is **not** zero-filled.
6. CSV round-trips convert everything to `float64` — re-cast with `.astype(int)` if needed.

---

<div align="center"><a href="./README.md">🏠 Back to the track</a> · <a href="../README.md">🧪 Lab home</a></div>
