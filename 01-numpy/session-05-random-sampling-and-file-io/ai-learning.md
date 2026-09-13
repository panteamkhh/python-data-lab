# 🤖 AI-Assisted Learning — Session 5: Random Sampling & File I/O

> Randomness and persistence are where "it works on my machine" bugs are born. AI helps you design reproducible experiments and understand the data-type traps.

---

## 🎯 Mission

Finish this session able to **sample from named distributions**, **make randomness reproducible with a seed**, **explain convergence of sample statistics**, and **round-trip arrays through `.npz` and `.csv` while knowing the dtype trade-offs**.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a statistics + NumPy mentor. Topic: <topic>.
  1. Intuition, 2. Formal explanation, 3. Minimal runnable example.
Then ask me one question that checks whether I understand the trade-off involved.
```

---

## 🔍 Explore with AI

1. "Explain the Law of Large Numbers using a simulation I can run in NumPy — no formulas first, then the math."
2. "When should I use `np.random.default_rng(seed)` (the modern Generator API) instead of `np.random.seed`? Show both."
3. "Compare `savez` vs `savez_compressed` — size, speed, and when each is worth it."
4. "Why does a CSV round-trip turn my integers into floats, and how do I check and fix the dtype after `np.loadtxt`?"

---

## 🛠️ Build with AI

Grow the **simulate–persist–round-trip** mini project into a reusable helper:

1. Write the simulation and both save/load paths yourself.
2. Ask: *"Refactor this into `save_npz(path, **arrays)` and `save_csv(path, arr, header)` helpers, and add a `verify_roundtrip` function using `np.array_equal`."*
3. Ask the AI to add a small Monte Carlo estimate (e.g. mean of a Pareto sample) and explain the convergence.

---

## 👀 AI Code Review

```text
Review my random-sampling and file-I/O code.
Flag: missing seeds, non-reproducible experiments, `.csv` dtype assumptions,
using `np.load` on text files (or `np.loadtxt` on `.npz`), and unsafe `allow_pickle` use.
Explain each risk in one line.
```

---

## 🐞 AI Debugging

Set up failures on purpose, then ask for the cause:

- `np.loadtxt("xy.csv")` without `delimiter=","` on a comma-separated file.
- Calling `np.load` on a plain `.csv`.
- Assuming `np.loadtxt` returns the same dtype you saved.

```text
Here is the error/output and the code. Explain what NumPy actually did and the correct fix.
```

---

## 🏆 AI Challenge

```text
Design an experiment where I must decide which distribution best models a given
real-world dataset (heights, income, waiting times). Give me the scenario and
ask me to defend my choice with sample statistics. Withhold the answer until I commit.
```

---

## 🪞 Reflection

- Why is a seed essential for science and for debugging?
- Which format would I choose for a 2 GB dataset, and why?
- What is one dtype bug I could now spot instantly?
