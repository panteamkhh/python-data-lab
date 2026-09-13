# 🧪 Exercises — Session 4: Vectorized Math & Grayscale Project

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Compute the square root and cube of every integer from 1 to 10 using vectorized NumPy functions (no loop).
2. **(Easy)** Compute `np.log(-5)` and `np.emath.log(-5)` and explain in one sentence why they differ.
3. **(Medium, coding)** Load any PNG/JPG image on your machine with Pillow + NumPy, print its shape, and print the mean value of each of its three color channels separately.
4. **(Medium)** Modify the grayscale project so that instead of the standard luminance weights, it uses simple equal weights (`1/3, 1/3, 1/3` for R, G, B). Save this as a separate file and (conceptually) compare it to the luminance-weighted version.
5. **(Advanced, conceptual)** Explain, step by step, why `img_data @ weights` — where `img_data` has shape `(460, 460, 3)` and `weights` has shape `(3,)` — is valid matrix multiplication and produces shape `(460, 460)`. What rule from linear algebra makes this work?

## 🚀 Mini Project — Black-and-White Threshold Tool

**Objective:** Extend the grayscale converter into a simple "black-and-white threshold" tool — a classic image-processing operation that turns a photo into pure black/white pixels (no gray at all), building directly on the grayscale array you already know how to produce.

**Steps:**

1. Reuse the grayscale array (`grayscale`, dtype `uint8`) from the Build It section.
2. Choose a threshold value, e.g. `128`.
3. Use **Boolean indexing / broadcasting** (from Session 3) to create a new array where every pixel becomes `255` (white) if its grayscale value is above the threshold, and `0` (black) otherwise.
4. Save the result as a new image and compare it visually with the grayscale version.
5. Experiment with different threshold values (e.g. 100 vs. 180) and observe how the image changes.
