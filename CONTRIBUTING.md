# Contributing

Thanks for helping improve **Python Data Lab**. Contributions of every size are welcome — typo fixes, clearer explanations, new exercises, or new mini projects.

## Ground rules

- **English only** in code comments and lesson content.
- **Keep the 5-file session structure:** `README.md`, `notebook.ipynb`, `exercises.md`, `exercises_solutions.ipynb`, `ai-learning.md`.
- **Every notebook must run top-to-bottom** with the versions in `requirements.txt`.
- **No external data files.** Generate datasets inline so the lab works offline.
- Match the existing tone: beginner-friendly, intuition → formal → code.

## Workflow

```bash
# 1. Fork and clone
git clone https://github.com/<your-username>/python-data-lab.git
cd python-data-lab

# 2. Create a branch
git checkout -b fix/pandas-session-03-typo

# 3. Make your change and run the affected notebooks
pip install -r requirements.txt
jupyter notebook

# 4. Commit and push
git add .
git commit -m "fix(pandas-03): correct dropna example"
git push origin fix/pandas-session-03-typo
```

Then open a Pull Request describing **what** changed and **why**.

## Commit messages

Use a short prefix so history stays readable:

| Prefix | For |
|---|---|
| `feat(<track>-<session>)` | new lesson content or exercises |
| `fix(...)` | corrections to content or code |
| `docs(...)` | README / cheat sheet / documentation |
| `chore` | tooling, ignore files, formatting |

## Reporting issues

Open an issue with the exact file, the command you ran, and the full error output. Screenshots are welcome for plotting problems.
