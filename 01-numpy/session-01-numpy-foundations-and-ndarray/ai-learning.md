# 🤖 AI-Assisted Learning — Session 1: NumPy Foundations & `ndarray`

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **what an `ndarray` is**, **why it is fast**, and **how `ndim`, `size`, and `shape` describe it**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a patient NumPy mentor. I am a beginner learning: <topic>.
Explain it in three layers:
  1. Intuition (one everyday analogy),
  2. Formal definition (technically correct),
  3. A minimal runnable NumPy example (<= 6 lines).
Then ask me 2 short questions to check whether I understood.
Do NOT solve my exercises for me unless I ask.
```

This three-layer pattern (intuition → formal → code) mirrors the `README.md` of this session and keeps the AI from drowning you in detail.

---

## 🔍 Explore with AI

Try these prompts and compare the AI's answer to what you already learned:

1. "Show me a 5-line benchmark comparing `np.arange(1_000_000) * 2` to a Python `for` loop. Why is NumPy faster?"
2. "Why must NumPy arrays be homogeneous? What would we lose if they could mix types?"
3. "Explain `np.empty` vs `np.zeros` in terms of what the computer actually does step by step."
4. "Give me the shape, `ndim`, and `size` of `np.zeros((3, 4, 2))` and explain your reasoning."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the `inspect_array` inspector — one step at a time:

1. Rewrite `inspect_array` yourself first.
2. Ask the AI: *"Add `nbytes`, `itemsize`, and `strides` to this inspector and explain what each one means for a `(2, 3, 4)` array."*
3. Run both versions on `np.zeros((2, 4, 3))` and confirm the numbers agree.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my NumPy code as a senior engineer.
Point out: non-idiomatic patterns, missed vectorization, and readability issues.
Give the improved version and explain each change in one line.
```

Common review findings in this session: using `np.array` when `np.arange` is clearer, forgetting the `np` alias, or hard-coding shapes instead of reading `.shape`.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: `np.array([[1, 2], [3, 4, 5]])` — ask the AI why NumPy prints a warning and what the resulting array looks like.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about ndarray creation and attributes.
Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting output is the fastest way to expose false confidence about `.shape` and `.size`.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `print()` before moving on?
