# 🤖 AI-Assisted Learning — Session 2: Plot Types & Choosing the Right Chart

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to **match a data question to the right chart type** and **draw each of the core plot types** with the object-oriented API. Your AI partner exists to pressure-test those choices, not to write code you could write yourself.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a patient Matplotlib mentor. I am a beginner learning: <topic>.
Explain it in three layers:
  1. Intuition (one everyday analogy),
  2. Formal definition (technically correct),
  3. A minimal runnable Matplotlib example (<= 6 lines, fig/ax style).
Then ask me 2 short questions to check whether I understood.
Do NOT solve my exercises for me unless I ask.
```

This three-layer pattern (intuition → formal → code) mirrors the `README.md` of this session and keeps the AI from drowning you in detail.

---

## 🔍 Explore with AI

Try these prompts and compare the AI's answer to what you already learned:

1. "I have one numeric variable measured in three groups. Compare a histogram, a box plot, and a violin plot — when would you choose each?"
2. "What does `density=True` change in `ax.hist`? Show the same data with `density=False` and `density=True` and explain the y-axis."
3. "Why are pie charts often criticized? Give me a dataset where a bar chart communicates the same thing more clearly."
4. "In `ax.errorbar`, what is the difference between symmetric `yerr` and passing a 2×N array? Show both."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the Distribution & Comparison figure — one step at a time:

1. Build the two-axes figure yourself first.
2. Ask the AI: *"Add a second overlaid histogram for another group with `alpha=0.5` and `density=True`, and adjust the bins so the two are directly comparable."*
3. Ask the AI to add vertical lines for the two group means with `ax.axvline`, then confirm the numbers by computing them yourself with NumPy.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my Matplotlib code as a senior engineer.
Point out: chart types that do not match the data's structure, missing density normalization,
hard-coded bin counts, overlapping legends, and confusing color choices.
Give the improved version and explain each change in one line.
```

Common review findings in this session: using a pie chart for many categories, comparing groups in separate histograms without shared bins, and forgetting `cap_size`/`capsize` on error bars.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: pass `tick_labels=` to `ax.boxplot` on an old Matplotlib version (or omit it and get no labels) — ask the AI how the box-label parameter changed across versions.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "which chart should I use?" questions about relationships, comparisons,
distributions, composition, and uncertainty.
Do not give the answers until I respond.
After I answer, reveal the correct chart type and explain any I got wrong.
```

Choosing the chart is the transferable skill; memorizing the call signature is secondary.

---

## 🪞 Reflection

Answer in your own notes:

- Which chart type do I reach for too often, even when it does not fit?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `plt.show()` before moving on?
