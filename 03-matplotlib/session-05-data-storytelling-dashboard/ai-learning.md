# 🤖 AI-Assisted Learning — Session 5: Data Storytelling & Dashboards

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **story editor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **how a pandas `DataFrame` becomes a Matplotlib figure**, **how a multi-panel dashboard is assembled**, and **what makes a chart honest and readable**. Your AI partner exists to critique your data story, not to write the story for you.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a data-storytelling mentor. I am a beginner learning: <topic>.
Explain it in three layers:
  1. Intuition (what the reader should notice first),
  2. Formal definition (the pandas/Matplotlib mechanics),
  3. A minimal runnable example (<= 10 lines, inline data only).
Then ask me which panel or annotation I would add and why.
Do NOT solve my exercises for me unless I ask.
```

This three-layer pattern (intuition → formal → code) mirrors the `README.md` of this session and keeps the AI focused on the message rather than the mechanics.

---

## 🔍 Explore with AI

Try these prompts and compare the AI's answer to what you already learned:

1. "Compare `ax.plot(df['month'], df['revenue'])` with `df.plot(x='month', y='revenue', ax=ax)`. When is each idiomatic, and what does pandas do for me in the second?"
2. "Explain `sharex` and `sharey` in `plt.subplots`. Give one case where sharing is correct and one where it silently misleads."
3. "Show me three ways to annotate the maximum of a time series, and explain the trade-offs of each."
4. "What are the most common ways a dashboard lies? List at least five and the honest alternative for each."

---

## 🛠️ Build with AI

Extend the session's Mini Project — the **Sales Dashboard** — one step at a time:

1. Build the inline `DataFrame` and the four panels yourself first.
2. Ask the AI: *"Suggest a different fourth panel that reveals something the other three hide, and explain the insight it adds."*
3. Try adding a fifth summary panel with `fig.add_axes` or a nested `GridSpec`, then ask the AI to explain the difference between `subplots`, `subplot_mosaic`, and `GridSpec`.
4. Export the dashboard as both PNG and SVG and compare file sizes and sharpness.

---

## 👀 AI Code Review

Paste **your own** dashboard code (not the AI's) and request a review:

```text
Review my dashboard code as a data-visualization lead.
Point out: panels that should share axes, truncated bar baselines,
missing units/sources, redundant ink, annotations that overlap, and a savefig
that is not print-ready.
Give the improved version and explain each change in one line.
```

Common review findings in this session: drawing four `plt.`-stateful charts instead of one `fig, axes` grid, forgetting `dpi` and `bbox_inches` in `savefig`, or starting a bar axis above zero.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Examples to try on purpose: indexing `axes[1]` after `plt.subplots(2, 2)` (the array is 2D), calling `df.plot(ax=ax)` on a `Series` with a `y=` argument, or saving with `bbox_inches="tight"` after `plt.show()` has already closed the figure — ask the AI why each fails.

---

## 🏆 AI Challenge

Ask the AI to generate a critique exercise, then verify every answer by actually redrawing the chart:

```text
Show me 5 deliberately misleading or cluttered charts (described in text or code).
For each, ask me to name the problem and propose a fix. Do not reveal the answers
until I respond. Then show your corrected version and explain each fix.
```

Critiquing a bad chart is the fastest way to internalize the best practices from the README.

---

## 🪞 Reflection

Answer in your own notes:

- Which storytelling tool — panels, annotations, or captions — improved my chart the most?
- Where did the AI help most, and where did its "improvement" actually add chartjunk?
- What is one claim in my dashboard I should verify with a real number before publishing it?
