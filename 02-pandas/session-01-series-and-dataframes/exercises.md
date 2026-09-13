# 🧪 Exercises — Session 1: Series & DataFrames

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

1. **(Easy)** Create a `Series` named `"temps"` from the list `[12, 15, 9, 18]` and print its `.name`, `.index`, and `.dtype`.
2. **(Easy)** Build a `Series` from the dictionary `{"apples": 3, "bananas": 5, "cherries": 7}`. What became the index? Then give it a custom index of `["A", "B", "C"]`.
3. **(Medium, coding)** Build a `DataFrame` from a dict of lists with columns `product`, `price`, and `units`. Print `df.shape`, `df.columns`, and `df.dtypes`.
4. **(Medium)** Create the same table from a *list of dicts*, then read the equivalent CSV text with `pd.read_csv(io.StringIO(...))`. Verify all three approaches produce the same values.
5. **(Advanced, conceptual)** Explain why `df["score"][0] = 99` is unsafe under pandas 3.0's Copy-on-Write, and write the idiomatic replacement using `.loc`.

## 🚀 Mini Project — Student Gradebook

**Objective:** Build a small "gradebook" DataFrame of students and their exam scores, inspect it, and derive a class average plus a letter grade for every student — all from data you create inline.

**Steps:**

1. Create a `DataFrame` from a dict of lists with columns `student`, `math`, and `science` (invent 5 students).
2. Add an `"average"` column as the row-wise mean of `math` and `science` (hint: `df[["math", "science"]].mean(axis=1)`).
3. Add a `"letter"` column using a helper function and `Series.map`: `>= 90` → `"A"`, `>= 80` → `"B"`, `>= 70` → `"C"`, else `"F"`.
4. Print `df.head()` and `df.describe()` to inspect the gradebook.
5. Select and display only the `student` and `average` columns.
6. Print the class average (`df["average"].mean()`) and count how many students passed (average `>= 70`).
