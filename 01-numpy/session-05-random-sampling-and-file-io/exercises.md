# 🧪 Exercises — Session 5: Random Sampling and File I/O

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Generate 20 random samples from a Normal distribution with mean `50` and standard deviation `10`. Print the sample mean and standard deviation.
2. **(Easy)** Generate 15 samples from a Uniform distribution between `0` and `1`, and count how many are above `0.5` (hint: combine with Session 3's boolean indexing).
3. **(Medium, coding)** Set a random seed, generate a 4×4 array of random normal values, save it to an `.npz` file under the key `"matrix"`, then write separate code that loads it back and confirms (with `np.array_equal`) that the reloaded array matches the original.
4. **(Medium)** Save a NumPy array of 5 floating-point numbers to a `.csv` file, then reload it and verify its dtype. Explain what you observe.
5. **(Advanced, conceptual)** Explain, in your own words, why increasing the sample size in `np.random.normal(mean, std, size=n)` causes the sample's own computed mean and standard deviation to converge toward the true `mean` and `std` parameters as `n` grows. What statistical principle does this illustrate?

## 🚀 Mini Project — Simulate, Persist, and Round-Trip a Dataset

**Objective:** Simulate a small dataset, persist it to disk in both formats covered in this lesson, and verify a full round-trip — a realistic workflow you'll repeat often when working with experimental or simulated data.

**Steps:**

1. Simulate `1000` samples of a fictional sensor reading using a Normal distribution with a mean and standard deviation of your choice, using a fixed seed for reproducibility.
2. Compute and print the sample mean and standard deviation, and compare them to your chosen true parameters.
3. Save the raw samples to a binary `.npz` file under the key `"sensor_readings"`.
4. Reload the `.npz` file into a new variable and confirm (via `np.array_equal`) it exactly matches the original array.
5. Separately, save the first 20 samples to a human-readable `.csv` file (with a header), reload them with `np.loadtxt`, and print both the reloaded values and their dtype.
