# 🤖 AI-Assisted Learning — Session 1: Figure/Axes Model & First Plots

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **what a `Figure` and an `Axes` are**, **why the object-oriented API is preferred**, and **how to style and save a plot**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

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

1. "Draw the Matplotlib object hierarchy — `Figure`, `Axes`, `Axis`, `Artist` — and explain how each one is nested inside the others."
2. "What exactly is the difference between the pyplot state machine and the object-oriented API? Show the same two-line plot in both styles."
3. "What do `figsize` and `dpi` actually control? If I set `figsize=(8, 4)` and `dpi=150`, how many pixels is the saved image?"
4. "Explain what `bbox_inches='tight'` changes when saving a figure, with a before/after description."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the styled function plot — one step at a time:

1. Write the `plot_function` helper yourself first and confirm it returns `(fig, ax)`.
2. Ask the AI: *"Turn this helper into `plot_functions(specs)` that draws several functions on the same axes from a list of `(func, label, color)` tuples and builds the legend automatically."*
3. Run both versions on `np.sin` and `np.cos` and confirm the curves and legend match.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my Matplotlib code as a senior engineer.
Point out: use of the state machine where the OO API is clearer, hard-coded colors or limits,
missing labels/legend, and clipping issues when saving.
Give the improved version and explain each change in one line.
```

Common review findings in this session: calling `plt.plot()` when multiple axes are involved, forgetting `label=` before `ax.legend()`, and saving with the default DPI when a higher-resolution image was needed.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: call `ax.savefig("out.png")` instead of `fig.savefig("out.png")` — ask the AI why the `Axes` object has no `savefig` and which object should own it.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about Figure vs Axes, figsize/dpi, xlim/ylim,
axis scales, and savefig options.
Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting how a plot changes is the fastest way to expose false confidence about the object model.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `plt.show()` before moving on?
