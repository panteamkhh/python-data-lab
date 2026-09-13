# 🧪 Exercises — Session 1: NumPy Foundations & ndarray

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Create a 1D NumPy array containing the even numbers from 0 to 20 (inclusive) using `np.arange`.
2. **(Easy)** Create a NumPy array of 7 evenly spaced numbers between -1 and 1 (inclusive) using `np.linspace`.
3. **(Medium, coding)** Create a 3D array of shape `(2, 2, 3)` using `np.array` with nested lists, then print its `ndim`, `size`, and `shape` to verify they match your expectation.
4. **(Medium)** Without running any code, predict the `.shape` of an array built from a list containing 5 lists, each containing 6 numbers. Then verify with code.
5. **(Advanced, conceptual)** Explain, in your own words, why `np.empty` can be faster than `np.zeros`, and describe one realistic scenario where using `np.empty` instead of `np.zeros` could introduce a subtle bug.

## 🚀 Mini Project — Array Attribute Inspector

**Objective:** Build a small "array attribute inspector" script that reports structural information about any array you give it — useful as a debugging habit whenever you're unsure about an array's shape.

**Steps:**

1. Write a function `inspect_array(arr)` that prints the array's `ndim`, `shape`, `size`, and `dtype`.
2. Test it against a 1D array made with `np.arange(10)`.
3. Test it against a 2D array made with `np.array([[1,2],[3,4],[5,6]])`.
4. Test it against a 3D array made with `np.zeros((2, 4, 3))`.
5. Add a check: if `arr.ndim == 1`, print `"This is a vector"`; if `arr.ndim == 2`, print `"This is a matrix"`; if `arr.ndim >= 3`, print `"This is a tensor"`.
