# 🧪 Exercises — Session 2: Reshaping and Indexing

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Create a 1D array of 16 elements using `np.arange`, then reshape it into a 4×4 matrix.
2. **(Easy)** Given `arr = np.arange(9).reshape(3, 3)`, write the indexing expression that returns the middle element (value `4`).
3. **(Medium, coding)** Create a 5×5 matrix using `np.arange(25).reshape(5, 5)`. Extract: (a) the last row, (b) the first column, (c) the 3×3 sub-block in the center.
4. **(Medium)** Explain what would happen (and why) if you tried `np.arange(10).reshape(3, 3)`.
5. **(Advanced, conceptual)** You are processing a very large array and need to flatten it just to *read* values (no mutation planned) as fast as possible. Which method should you choose — `reshape(-1)`, `ravel()`, or `flatten()` — and why? Now suppose you *do* need to mutate the flattened version without ever affecting the original array — which method changes your answer, and why?

## 🚀 Mini Project — Matrix Cropper

**Objective:** Build a small "matrix cropper" utility that extracts a specific rectangular region from any 2D array — a pattern used constantly in image processing and tabular data extraction.

**Steps:**

1. Create a 6×6 matrix using `np.arange(36).reshape(6, 6)`.
2. Write a function `crop(matrix, row_start, row_end, col_start, col_end)` that returns the sub-matrix using 2D slicing.
3. Use your function to extract the top-left 2×2 corner.
4. Use your function to extract the bottom-right 3×3 corner.
5. Flatten the top-left corner you extracted using `.flatten()` and confirm that modifying the flattened result does not change the original matrix.
