# 🤖 AI-Assisted Learning — Session 1: Series & DataFrames

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **what a `Series` is**, **what a `DataFrame` is**, and **how to inspect and select columns**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a patient pandas mentor. I am a beginner learning: <topic>.
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

1. "Compare a pandas `Series` to a NumPy array. What extra information does the Series carry?"
2. "Why does `df['a']` return a Series but `df[['a']]` return a DataFrame? Explain the bracket rule."
3. "Give me three ways to build the same DataFrame and tell me when each one is the most convenient."
4. "What does `df.describe()` do with a text column, and how do I include it?"

---

## 🛠️ Build with AI

Extend the session's Mini Project — the Student Gradebook — one step at a time:

1. Rebuild the gradebook and the `average` / `letter` columns yourself first.
2. Ask the AI: *"Add a `rank` column that orders students by average without changing row order, and explain how `Series.rank(ascending=False)` works."*
3. Recompute both versions and confirm the ranks agree.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my pandas code as a senior engineer.
Point out: non-idiomatic patterns, chained assignment, missed vectorization, and readability issues.
Give the improved version and explain each change in one line.
```

Common review findings in this session: using `df["score"][0] = 99` instead of `df.loc[0, "score"] = 99`, using `inplace=True`, or building a DataFrame row by row instead of from a dict of lists.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: assign a list of the wrong length to a new column (`df["x"] = [1, 2]` on a 5-row frame) and ask the AI why pandas raises a length-mismatch error.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about Series/DataFrame construction and inspection.
Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting output is the fastest way to expose false confidence about `.shape`, `.dtypes`, and the single-vs-double bracket rule.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `print()` before moving on?
