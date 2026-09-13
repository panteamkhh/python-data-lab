# 🤖 AI-Assisted Learning — Session 4: Styling & Colormaps

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **design tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **how Matplotlib resolves a color**, **what `alpha` actually controls**, and **which colormap family fits which kind of data**. Your AI partner exists to pressure-test your visual judgment, not to pick colors you could pick yourself.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a patient Matplotlib mentor. I am a beginner learning: <topic>.
Explain it in three layers:
  1. Intuition (one everyday visual analogy),
  2. Formal definition (technically correct),
  3. A minimal runnable Matplotlib example (<= 8 lines).
Then ask me 2 short questions about which styling choice I would make and why.
Do NOT solve my exercises for me unless I ask.
```

This three-layer pattern (intuition → formal → code) mirrors the `README.md` of this session and keeps the AI from dumping every option on you at once.

---

## 🔍 Explore with AI

Try these prompts and compare the AI's answer to what you already learned:

1. "List every way a Matplotlib color can be specified — named, hex, RGBA tuple, `'C0'` — and explain when each form is clearest."
2. "Why is `alpha` more about *data density* than about decoration? Show a scatter plot where alpha changes the interpretation."
3. "Explain the difference between sequential, diverging, cyclic, and qualitative colormaps with one real dataset example each."
4. "What is a perceptually uniform colormap, and why do `viridis` and `cividis` matter for color-blind readers?"

---

## 🛠️ Build with AI

Extend the session's Mini Project — the **Chart Makeover** — one step at a time:

1. Write the plain "before" chart yourself first.
2. Ask the AI: *"Suggest three distinct style-sheet + color combinations for this chart, and explain the mood each one communicates."*
3. Apply one combination, then ask: *"Add `ax.fill_between` under the curve and explain how `alpha` interacts with the grid behind it."*
4. Run both versions and compare them side by side before deciding.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my Matplotlib styling code as a data-visualization engineer.
Point out: hard-coded values that should be rcParams, inaccessible color choices,
colormaps mismatched to the data type, and any missing labels or limits.
Give the improved version and explain each change in one line.
```

Common review findings in this session: using `plt.` stateful calls instead of `fig, ax`, choosing `'jet'` for sequential data, or applying a style sheet globally with `plt.style.use` when a `with plt.style.context(...)` block would be safer.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Examples to try on purpose: pass a 4-element list `[0.1, 0.2, 0.3, 0.4]` where a single color is expected, call `fig.colorbar()` before any mappable exists, or set `shading="auto"` on an `imshow` call — ask the AI why each one fails and which function actually owns that argument.

---

## 🏆 AI Challenge

Ask the AI to generate a design quiz, then verify every answer by actually rendering the chart:

```text
Create 5 questions that show a dataset description and ask me to choose the
right colormap family and alpha value. Do not give the answers until I respond.
After I answer, reveal the best choice and explain any I got wrong.
```

Defending a colormap choice out loud is the fastest way to expose guesswork about sequential vs. diverging data.

---

## 🪞 Reflection

Answer in your own notes:

- Which styling dial could I now teach to a friend?
- Where did the AI help most, and where did its color advice actually look worse?
- What is one default `rcParams` change I should verify with a real rendered figure before moving on?
