# 🤖 AI-Assisted Learning — Session 3: Cleaning Data

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **how missing values are detected and handled**, **why dtype conversion can fail silently**, and **how duplicates and messy text are cleaned**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

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

1. "Compare `astype('float64')` and `pd.to_numeric(..., errors='coerce')` on a messy column. When does each fail?"
2. "Why does an integer column become `float64` when it contains a missing value? What is a nullable `Int64`?"
3. "When should I `dropna` versus `fillna`? Walk through the trade-offs for a survey dataset."
4. "Explain the `category` dtype in terms of storage: codes plus a lookup table."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the Messy CSV Cleanup — one step at a time:

1. Clean the CSV yourself first, up to step 6.
2. Ask the AI: *"Extract a `first_name` column from `name` with `.str.split(..., expand=True)`, and a `domain` column from an email with `.str.split('@', expand=True)`."*
3. Re-run the pipeline and confirm the new derived columns look right.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my pandas cleaning code as a senior engineer.
Point out: inplace misuse, chained assignment, silent dtype bugs, and unassigned results.
Give the improved version and explain each change in one line.
```

Common review findings in this session: calling `df.dropna(inplace=True)` and ignoring Copy-on-Write, using `astype` on messy text, or forgetting `na=False` in `.str.contains`.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: run `pd.to_numeric(pd.Series(["1", "n/a", "3"]))` and ask the AI why rows containing `"n/a"` raise instead of becoming `NaN`, then try `errors="coerce"`.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about isna, fillna, to_numeric, and drop_duplicates.
Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting output is the fastest way to expose false confidence about dtype changes and duplicate handling.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `print()` before moving on?
