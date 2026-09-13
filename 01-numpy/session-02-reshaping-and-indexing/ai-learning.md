# 🤖 AI-Assisted Learning — Session 2: Reshaping, Views, and Indexing

> Use an AI coding assistant as a **thinking partner**. The trap in this session is the **view vs. copy** distinction — AI is excellent at generating *predictions* for you to verify.

---

## 🎯 Mission

Finish this session able to **reshape any array**, **flatten it three different ways**, and predict **whether a result is a view or a copy** before running the code. If you can predict, you understand.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a NumPy mentor. Topic: <topic>.
  1. Give me the intuition,
  2. the formal rule,
  3. a runnable example.
Then ask me to predict the output of one variation before revealing it.
Do NOT give the answer until I try.
```

---

## 🔍 Explore with AI

1. "Explain the difference between `reshape(-1)`, `ravel()`, and `flatten()` in terms of memory, not just output."
2. "Which operations return views and which return copies? Give me a table of at least 8 NumPy operations."
3. "Why can `reshape` only return a view *when possible*? When is it forced to copy?"
4. "Show me a case where mutating a slice of an array silently changes the original, and explain why."

---

## 🛠️ Build with AI

Grow the session's **matrix cropper** into a small utility:

1. Implement `crop(...)` yourself.
2. Ask: *"Add a `view=False` parameter so the crop returns a copy when requested, and explain which NumPy call makes that happen."*
3. Test it: mutate the returned crop, then check whether the source matrix changed.

---

## 👀 AI Code Review

```text
Review my indexing/reshaping code.
Flag: hidden copies, unnecessary `.flatten()`, chained `[i][j]` indexing that should be `[i, j]`, and off-by-one slices.
Then show a more idiomatic version.
```

---

## 🐞 AI Debugging

Paste the error and the smallest snippet:

```text
Error: <traceback>
Code: <code>
What is the root cause? Show the fix but don't rewrite my whole file.
```

Deliberately break things and study the errors: `np.arange(10).reshape(3, 3)`, `a = np.arange(5); a[2:] = np.arange(3)`, and `b[:, 5]` on a matrix with 4 columns.

---

## 🏆 AI Challenge

```text
Give me 6 view-vs-copy puzzles.
For each: show code, ask whether the original array is modified, and hide the answer
until I respond. Then explain each result using the term "shares memory".
```

---

## 🪞 Reflection

- Can I explain view vs. copy in one sentence without using the word "reference"?
- Did I ever mutate an array by accident this session?
- What is the mental picture I now keep for `array[rows, columns]`?
