---
title: "Python Virtual Environments: From venv to Poetry"
categories:
  - Python
  - Development
tags:
  - python
  - virtualenv
  - poetry
  - dependency-management
date: 2024-08-22
---

Managing Python dependencies cleanly is essential for reproducible projects. Here's a rundown of the options from the built-in `venv` to modern tools like Poetry.

## venv — The Built-in Option

Python 3.3+ ships with `venv`, and it's still a solid choice:

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
pip install -r requirements.txt
```

**Pros:** No extra tools needed. **Cons:** Manual dependency management, no lock file.

## pip-tools — A Step Up

`pip-tools` adds lock-file support:

```bash
pip install pip-tools
pip-compile requirements.in   # generates requirements.txt with pinned versions
pip-sync requirements.txt     # syncs venv to exactly match the lock file
```

## Poetry — The Modern Standard

Poetry handles dependencies, packaging, and publishing:

```bash
poetry new my-project
poetry add requests
poetry add --group dev pytest black
```

The `pyproject.toml` keeps everything declarative, and `poetry.lock` ensures reproducible installs across machines and CI.

## Quick Comparison

| Tool | Lock File | Package Publishing | Learning Curve |
|------|-----------|-------------------|---------------|
| venv + pip | No | Manual | Low |
| pip-tools | Yes | Manual | Low |
| Poetry | Yes | Built-in | Medium |
| PDM | Yes | Built-in | Medium |

## My Recommendation

For personal projects, start with **venv + pip**. Once you need reproducibility (CI, team, deployment), switch to **Poetry** — it's become the de facto standard and has the best ecosystem support.

The important thing is to always isolate your project dependencies. Never `pip install` globally.
