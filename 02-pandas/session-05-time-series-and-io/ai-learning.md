# 🤖 AI-Assisted Learning — Session 5: Time Series & I/O

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **how a `DatetimeIndex` unlocks `resample` and rolling operations**, **the difference between `shift`, `diff`, and `pct_change`**, and **which file format to choose for which job**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a patient pandas time-series mentor. I am a beginner learning: <topic>.
Explain it in three layers:
  1. Intuition (one everyday analogy),
  2. Formal definition (technically correct),
  3. A minimal runnable pandas example (<= 6 lines).
Then ask me 2 short questions to check whether I understood.
Do NOT solve my exercises for me unless I ask.
```

This three-layer pattern (intuition → formal → code) mirrors the `README.md` of this session and keeps the AI from drowning you in detail.

---

## 🔍 Explore with AI

Try these prompts and compare the AI's answer to what you already learned:

1. "Explain `resample` as a `groupby` over time. Show why it needs a `DatetimeIndex` and what error I get without one."
2. "Compare `resample('W')` with `rolling(7)`: disjoint versus overlapping windows. Give a 10-row example where the two results visibly differ."
3. "Break down `shift`, `diff`, and `pct_change` on the same Series and show that `diff()` equals `x - x.shift(1)`."
4. "When should I save to CSV, Excel, or Parquet? Compare dtype preservation, file size, and read speed, and mention the required engines."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the **Daily Sales Time Series** — one step at a time:

1. Generate the series, resample it, and compute the rolling mean yourself first.
2. Ask the AI: *"Refactor this into `load_sales(path)` and `save_all(df, stem)` helpers, using Parquet as the default and CSV/Excel as exports."*
3. Ask it to add a text-only monthly seasonality summary (`resample("ME").agg(["sum", "mean"])`) and explain any frequency-alias changes between pandas versions.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my time-series and file-I/O code as a senior data engineer.
Flag: missing pd.to_datetime or parse_dates, unsorted DatetimeIndex, resample on a
non-datetime index, timezone-naive assumptions, writing the index when it should be
reset, and forgetting required engines (openpyxl / pyarrow).
Give the improved version and explain each change in one line.
```

Common review findings in this session: comparing timestamps stored as strings, forgetting `sort_index()`, mixing `resample("M")` (deprecated) with `"ME"`, and assuming CSV preserves dtypes.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Examples to try on purpose:

- Calling `.resample("W")` on a DataFrame whose index is still an integer.
- Calling `.dt.year` on a string column that was never converted with `pd.to_datetime`.
- Reading a CSV back and wondering why `df["date"]` sorts alphabetically instead of chronologically.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about resample, rolling, shift/diff/pct_change,
and file-format round-trips. Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting which leading values of a rolling window are `NaN` is the fastest way to expose false confidence about windows.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend — `resample`, `rolling`, or the `.dt` accessor?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `print()` before moving on?
