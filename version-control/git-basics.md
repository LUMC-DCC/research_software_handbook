---
title: Git Basics
---

# Git Basics

Git tracks changes to files by storing a history of *snapshots* called commits. This chapter gets you from zero to a working local Git workflow.

## Setup

Install Git from [git-scm.com](https://git-scm.com/) or via your package manager. Then tell Git who you are — this information is attached to every commit you make:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "nano"   # or vim, code --wait, etc.
```

Verify your configuration:

```bash
git config --list
```

## Core concepts

Understanding three areas clears up most Git confusion:

```{figure} ../_static/git-areas.png
:alt: Diagram showing the working directory, staging area, and repository
:align: center

The three areas of a Git project. Changes flow from the working directory → staging area → repository.
```

**Working directory**
: The folder on your disk where you edit files. Git sees these changes but does not record them until you ask it to.

**Staging area (index)**
: A holding area where you assemble the changes that will go into your next commit. This lets you craft clean, logical commits even when you have made messy edits.

**Repository (`.git` folder)**
: The database of all commits, branches, and history. It lives inside your project folder as a hidden `.git` directory.

## Initialising a repository

To start tracking an existing project:

```bash
cd my-project/
git init
```

Or clone an existing remote repository:

```bash
git clone https://github.com/user/repo.git
cd repo/
```

## The everyday workflow

### Check what has changed

```bash
git status
```

`git status` is safe to run any time — it never changes anything. Get in the habit of running it before every commit.

### Stage changes

Add a specific file:

```bash
git add analysis.py
```

Add all changes in the current directory:

```bash
git add .
```

Review exactly what is staged before committing:

```bash
git diff --staged
```

### Commit

```bash
git commit -m "Add normalisation step to preprocessing pipeline"
```

A good commit message completes the sentence *"If applied, this commit will…"*. Keep the subject line under 72 characters. For complex changes, add a body:

```bash
git commit
```

(This opens your configured editor.)

### View history

```bash
git log
git log --oneline          # compact view
git log --oneline --graph  # visualise branches
```

### Compare versions

```bash
git diff                   # unstaged changes vs last commit
git diff --staged          # staged changes vs last commit
git diff HEAD~1            # current state vs one commit ago
```

## Undoing things

```{warning}
The commands below modify history or discard changes. Read what each one does before running it.
```

**Unstage a file** (file stays edited on disk):

```bash
git restore --staged analysis.py
```

**Discard edits in the working directory** (irreversible — file goes back to last commit):

```bash
git restore analysis.py
```

**Amend the last commit** (message or staged files; only safe before pushing):

```bash
git commit --amend
```

**Revert a commit** (creates a new commit that undoes a previous one — safe to use on shared history):

```bash
git revert abc1234
```

## Ignoring files

Create a `.gitignore` file in your project root to tell Git which files to ignore. Common patterns for research projects:

```text
# Python
__pycache__/
*.pyc
.venv/

# Jupyter
.ipynb_checkpoints/

# Data (track metadata, not large raw files)
data/raw/
*.csv
*.tsv
*.h5

# Operating system
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
```

Commit your `.gitignore` so that everyone on the team benefits from it.

## Summary: command reference

```{list-table}
:header-rows: 1
:widths: 35 65

* - Command
  - What it does
* - `git init`
  - Initialise a new repository in the current folder
* - `git clone <url>`
  - Copy a remote repository to your machine
* - `git status`
  - Show the state of the working directory and staging area
* - `git add <file>`
  - Stage a file (or `.` for all changes)
* - `git diff --staged`
  - Show exactly what will go into the next commit
* - `git commit -m "..."`
  - Save staged changes as a commit with a message
* - `git log --oneline`
  - Show the commit history in compact form
* - `git restore --staged <file>`
  - Unstage a file without discarding edits
* - `git restore <file>`
  - Discard edits in the working directory (irreversible)
* - `git revert <hash>`
  - Create a new commit that undoes a previous commit
```

---

Next: {doc}`github-flow` — push your repository to GitHub and start collaborating.
