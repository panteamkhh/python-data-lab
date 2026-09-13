# 🤖 AI-Assisted Learning — Session 3: Subplots, Layouts & Annotations

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to **arrange multiple panels** in grids and custom layouts, **share axes** for honest comparisons, and **annotate** a figure so it explains itself. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

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

1. "Compare `tight_layout()`, `constrained_layout`, and manual `subplots_adjust`. When should I use each, and which is the modern default?"
2. "Show me three `GridSpec` layouts with row and column spans: a hero-plus-thumbnails dashboard, a sidebar layout, and a calendar-like grid."
3. "What exactly does `twinx()` return, and how do I merge the legends from both axes into one box?"
4. "Explain the difference between `ax.text` with default coordinates and `ax.text` with `transform=ax.transAxes`."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the 2×2 dashboard — one step at a time:

1. Build the four-panel dashboard yourself first.
2. Ask the AI: *"Refactor the dashboard so each panel is produced by a small helper function `panel_xxx(ax, data)` that only draws, never creates the figure."*
3. Ask the AI to replace the uniform 2×2 grid with a `GridSpec` where the top-left panel spans the first row, then verify the layout still saves cleanly with `bbox_inches="tight"`.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my Matplotlib multi-panel code as a senior engineer.
Point out: repeated styling that should be a loop or helper, panels with mismatched limits,
missing shared axes where comparison is intended, overlapping titles/labels, and
annotations that do not actually point at the data.
Give the improved version and explain each change in one line.
```

Common review findings in this session: creating figures inside helper functions, forgetting `fig.tight_layout()`, and annotating with hard-coded coordinates that break when the data changes.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Example to try on purpose: index a `2 × 2` grid with `axes[3]` instead of `axes.ravel()[3]` — ask the AI why the 2D array throws an error and how `ravel()` changes the indexing.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the layout" questions about subplots grids, sharex/sharey,
GridSpec spans, twinx, and annotation coordinates.
Do not give the answers until I respond.
After I answer, reveal the correct layout and explain any I got wrong.
```

Predicting a layout from code is the fastest way to master the `Axes` array and `GridSpec` subscripts.

---

## 🪞 Reflection

Answer in your own notes:

- Which layout tool — `subplots`, `GridSpec`, or `twinx` — do I now understand well enough to teach?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `plt.show()` before moving on?
