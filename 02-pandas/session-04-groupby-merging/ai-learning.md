# 🤖 AI-Assisted Learning — Session 4: GroupBy & Merging

> Use an AI coding assistant (ChatGPT, Claude, Copilot Chat, Gemini, …) as a **tutor**, not an answer machine. Work through the sections in order — each one builds on the previous.

---

## 🎯 Mission

Finish this session able to explain, without notes, **how `groupby` splits, applies, and combines**, **when to use `agg` versus `transform`**, and **what each `how=` mode of `merge` does to unmatched keys**. Your AI partner exists to pressure-test that understanding, not to write code you could write yourself.

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

1. "Show me `groupby().agg()` with a list, a dict, and named aggregation on the same tiny DataFrame. Which style produces `MultiIndex` columns, and why?"
2. "Walk me through `agg` versus `transform` on a `groupby`, including the shape of the result from each. When must I use `transform`?"
3. "Demonstrate `pd.crosstab`, `Series.value_counts`, and `pivot_table` on one DataFrame and explain how the three results differ."
4. "Explain the four `how=` modes of `merge` as a Venn diagram in words: inner, left, right, outer. Which one can increase the row count, and when?"

---

## 🛠️ Build with AI

Extend the session's Mini Project — the **Orders + Customers** pipeline — one step at a time:

1. Write the `merge` and the regional `agg` yourself first.
2. Ask the AI: *"Refactor this into a function `summarize_by_region(orders, customers)` and add a `validate="many_to_one"` check so the merge fails loudly on bad keys."*
3. Ask it to add a `join` against a small `targets` table (indexed by `region`) that flags regions below target, and explain when `join` is more convenient than `merge`.

---

## 👀 AI Code Review

Paste **your own** solution (not the AI's) and request a review:

```text
Review my pandas groupby/merge code as a senior data engineer.
Point out: non-idiomatic aggregation, unnecessary merges, accidental row explosion,
MultiIndex columns that should be flattened, and any reliance on chained assignment.
Give the improved version and explain each change in one line.
```

Common review findings in this session: using a `for` loop over unique values instead of `groupby`, using `merge` when `transform` was the real intent, forgetting `validate=`, and leaving `NaN`-filled cells where `fill_value=0` was meant.

---

## 🐞 AI Debugging

When something breaks, paste the **traceback plus the smallest snippet** that reproduces it:

```text
I got this error: <error>
Here is the minimal code: <code>
Explain the root cause, then give me the fix — but let me type it myself.
```

Examples to try on purpose:

- Merging on a key whose dtypes differ (e.g. integer `customer_id` versus string `"101"`) and getting **zero** matches.
- A `how="inner"` merge that silently drops rows, then discovering `validate="one_to_many"` would have warned you.
- Calling `.transform` with a function that changes length — and reading the error pandas raises.

---

## 🏆 AI Challenge

Ask the AI to generate a quiz, then verify every answer by running the code:

```text
Create 5 "predict the output" questions about groupby, agg vs transform, and merge
how= modes. Do not give the answers until I respond.
After I answer, reveal the correct output and explain any I got wrong.
```

Predicting how many rows survive a merge is the fastest way to expose false confidence about joins.

---

## 🪞 Reflection

Answer in your own notes:

- Which concept could I now teach to a friend — `groupby`, `transform`, or `merge`?
- Where did the AI help most, and where did it actually confuse me?
- What is one thing I still need to verify with a real `print()` before moving on?
