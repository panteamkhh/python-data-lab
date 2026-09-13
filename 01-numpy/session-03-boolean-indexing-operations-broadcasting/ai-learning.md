# 🤖 AI-Assisted Learning — Session 3: Boolean Indexing, Operations & Broadcasting

> This is the session where AI-generated practice pays off most. Broadcasting and operator precedence are full of subtle traps — let the AI invent variations and you predict the outcome.

---

## 🎯 Mission

Finish this session able to **filter arrays with Boolean masks**, **combine conditions safely**, **tell element-wise multiplication from matrix multiplication**, and **apply the broadcasting rule to predict result shapes** without running code.

---

## 🧩 Prompt Pattern (reuse in every session)

```text
Act as a NumPy mentor. Topic: <topic>.
  1. Intuition, 2. Formal rule, 3. Runnable example.
Then quiz me with one "what shape will this produce?" question.
Withhold the answer until I respond.
```

---

## 🔍 Explore with AI

1. "Explain broadcasting as *aligning trailing dimensions*, with 3 examples that fail and 3 that succeed."
2. "Why do I need parentheses in `(a > 2) & (a < 8)`? What does Python evaluate first without them?"
3. "When should I use `np.where` instead of Boolean indexing? Give a concrete example."
4. "Show that `a * b` and `a @ b` differ for 2×2 matrices, and write out the arithmetic for one cell by hand."

---

## 🛠️ Build with AI

Extend the session's **grade filter and curve** tool:

1. Build it yourself with `np.random.randint`, Boolean masks, and normalization.
2. Ask: *"Add letter grades using `np.select` or `np.digitize`, then show the distribution of grades without a loop."*
3. Compare your implementation with the AI's and justify every difference.

---

## 👀 AI Code Review

```text
Review my Boolean-indexing and broadcasting code.
Flag: missing parentheses around conditions, `and`/`or` used instead of `&`/`|`,
`a * b` where `a @ b` was intended, and any manual `for` loop that broadcasting could remove.
Suggest the idiomatic replacement for each.
```

---

## 🐞 AI Debugging

Try these failing snippets, then ask the AI to explain the **exact** error:

- `a[a > 2 & a < 8]` — precedence bug.
- `np.array([1, 2, 3]) + np.array([1, 2])` — incompatible trailing dimensions.
- `np.array([1, 2]) @ np.array([1, 2])` — 1-D operands to `matmul`.

```text
Explain why each error happens using the broadcasting/matmul rules.
Then tell me the smallest change that fixes it.
```

---

## 🏆 AI Challenge

```text
Give me 8 "predict the shape (or the error)" broadcasting puzzles with mixed
scalars, (M,1), (1,N), (N,), and (M,N) shapes. Hide answers until I respond,
then explain each result by aligning dimensions from the right.
```

---

## 🪞 Reflection

- Can I state the broadcasting rule from memory?
- Do I now default to `@` for matrix multiplication and `*` only for element-wise work?
- What did the precedence bug teach me about Python operator order?
