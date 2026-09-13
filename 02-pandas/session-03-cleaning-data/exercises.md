# 🧪 Exercises — Session 3: Cleaning Data

Try to solve these on your own first. Solutions are in [`exercises_solutions.ipynb`](./exercises_solutions.ipynb).

Build the messy starter frame inline for every exercise:

```python
import io
import numpy as np
import pandas as pd

messy = pd.read_csv(io.StringIO("""name,city,price,units,joined
  ana ,berlin,12.5,3,2023-01-05
BO,Paris,7.5,,2023-02-10
Cara,lisbon,n/a,5,not-a-date
dan,BERLIN,22.0,2,2023-03-15
ana,berlin,12.5,3,2023-01-05
Eve,,15.0,4,2023-04-01
"""))
```

1. **(Easy)** Print `messy.isna().sum()` and `messy.dtypes`. Which columns have missing values, and which columns are not the type you would expect?
2. **(Easy)** Trim whitespace and normalize the case of the `name` and `city` columns using `.str` (names to title case, cities to title case).
3. **(Medium, coding)** Convert `price` to numeric and `joined` to datetime using the `errors="coerce"` variants. Then fill missing `price` with the column mean and missing `units` with `0`.
4. **(Medium)** Remove the exact duplicate `ana` row, and convert `city` to the `category` dtype. Print `messy.duplicated().sum()` before and after.
5. **(Advanced, conceptual)** Explain the difference between `astype("float64")` and `pd.to_numeric(..., errors="coerce")` on a messy column, and describe a scenario where the wrong choice silently corrupts your analysis.

## 🚀 Mini Project — Messy CSV Cleanup

**Objective:** Take a deliberately broken CSV string and clean it into a tidy, typed, duplicate-free DataFrame — reporting what changed at each step.

**Steps:**

1. Load the CSV below with `pd.read_csv(io.StringIO(...))`:

   ```text
   name,city,signup,spend,visits
     Ana ,berlin,2023-01-05,12.50,3
   BO,Paris,2023-02-10,7.5,
   Cara,lisbon,not-a-date,n/a,5
   dan,BERLIN,2023-03-15,22.00,2
   Ana,berlin,2023-01-05,12.50,3
   Eve,,2023-04-01,15.0,4
   ```

2. Rename `signup` to `joined` and `spend` to `amount`.
3. Clean the text columns: strip and title-case `name`; strip and title-case `city`; replace the empty city with `"Unknown"` (hint: `.replace("", np.nan)` then `fillna`).
4. Convert `joined` with `pd.to_datetime(..., errors="coerce")` and `amount` with `pd.to_numeric(..., errors="coerce")`.
5. Report missing values per column with `isna().sum()`, then fill missing `visits` with `0` and missing `amount` with the column mean.
6. Count and drop duplicate rows (consider only `name` and `joined` as the key), keeping the first occurrence.
7. Convert `city` to `category` and print `df.dtypes` plus the final clean frame.
