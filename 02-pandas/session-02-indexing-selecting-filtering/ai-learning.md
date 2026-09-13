# 🤖 AI-Assisted Learning — Session 2: Indexing, Selecting & Filtering

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **when to use `.loc` vs `.iloc`**, **how boolean masks combine**, and **why `.loc` assignment is Copy-on-Write-safe**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

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

1. "Why is `df.loc['r1':'r3']` inclusive but `df.iloc[0:3]` exclusive? Explain the design reasoning."
2. "Why must `&` and `|` conditions be wrapped in parentheses? Show what breaks without them."
3. "Compare `df[df['x'].between(10, 20)]`, `df.query('10 <= x <= 20')`, and a hand-written mask."
4. "What is a boolean mask internally, and why does it stay aligned with the DataFrame?"

---

## 🛠️ Build with AI

Extend the session's Mini Project — the Sales Explorer — one step at a time:

1. Rebuild the sales table and answer steps 4–7 yourself first.
2. Ask the AI: *"Rewrite my boolean filters using `query()` and `eval()`, then explain the performance and readability trade-offs."*
3. Run both versions and confirm the filtered row counts match exactly.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my pandas filtering code as a senior engineer.
Point out: missing parentheses, chained assignment, misuse of and/or, and readability issues.
Give the improved version and explain each change in one line.
```

Common review findings in this session: using Python `and`/`or` instead of `&`/`|`, forgetting parentheses, or updating a filter with `df["col"][mask] = ...` instead of `df.loc[mask, "col"] = ...`.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: run `df[df["price"] > 10 and df["units"] > 100]` and ask the AI why pandas raises `ValueError: The truth value of a Series is ambiguous`.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about loc, iloc, slicing, and boolean filters.
Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting output is the fastest way to expose false confidence about inclusive vs. exclusive slices.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `print()` before moving on?
