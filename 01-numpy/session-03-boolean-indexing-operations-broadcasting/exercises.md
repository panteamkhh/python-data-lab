# 🧪 Exercises — Session 3: Boolean Indexing, Operations, and Broadcasting

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Given `arr = np.arange(20)`, return only the elements divisible by 4.
2. **(Easy)** Given `arr = np.arange(20)`, return elements that are either less than 3 or greater than 15 (use `|`).
3. **(Medium, coding)** Create two 3×3 matrices with `np.arange(9).reshape(3,3)` and `np.arange(9, 18).reshape(3,3)`. Compute both their element-wise product and their true matrix product, and print both, clearly labeled.
4. **(Medium)** Given a `(5, 1)` column array and a `(1, 4)` row array, predict the shape of their sum *before* running code, then verify.
5. **(Advanced, conceptual)** Explain, using the broadcasting rule, why `np.array([1,2,3]) + np.array([1,2])` fails, but `np.array([[1,2,3],[4,5,6]]) + np.array([1,2,3])` succeeds. What shape does the second result have?

## 🚀 Mini Project — Grade Filter and Curve

**Objective:** Build a small "grade filter and curve" tool that combines everything from this lesson: boolean filtering, element-wise math, and broadcasting-based normalization.

**Steps:**

1. Create an array of 20 simulated exam scores (integers between 40 and 100) using `np.random.randint` (or hard-code a list).
2. Use boolean indexing to find how many students scored below 60 ("at risk" students).
3. Use boolean indexing (with `&`) to find students who scored between 70 and 85 inclusive.
4. "Curve" the grades using broadcasting: add a flat 5 points to every score, capped at 100 using `np.minimum`.
5. Normalize the *original* (uncurved) scores to a mean of 0 and standard deviation of 1, and print the result.
