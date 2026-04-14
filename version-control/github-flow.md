---
title: GitHub Flow
---

# GitHub Flow

GitHub Flow is a lightweight workflow designed for teams that collaborate through pull requests. It keeps the `main` branch always deployable and makes it easy to trace *why* every change was made.

## Pushing to GitHub

### First-time setup: add a remote

After creating a new repository on GitHub:

```bash
git remote add origin https://github.com/your-org/your-repo.git
git branch -M main
git push -u origin main
```

The `-u` flag sets `origin/main` as the default upstream, so future pushes need only `git push`.

### Subsequent pushes

```bash
git push
```

### Pulling changes made by others

```bash
git pull
```

`git pull` is shorthand for `git fetch` (download changes) followed by `git merge` (integrate them). If you prefer to keep a linear history:

```bash
git pull --rebase
```

## Working with branches

Branches let you work on a feature or fix without affecting `main`.

```{mermaid}
gitGraph
   commit id: "Initial commit"
   commit id: "Add data loader"
   branch feature/normalisation
   checkout feature/normalisation
   commit id: "Add z-score normalisation"
   commit id: "Add unit tests"
   checkout main
   merge feature/normalisation id: "Merge PR #4"
   commit id: "Fix typo in README"
```

### Create and switch to a branch

```bash
git checkout -b feature/normalisation
# or, with newer Git:
git switch -c feature/normalisation
```

### List branches

```bash
git branch          # local branches
git branch -r       # remote branches
git branch -a       # all
```

### Push a branch to GitHub

```bash
git push -u origin feature/normalisation
```

### Merge a branch locally (after review)

```bash
git checkout main
git merge feature/normalisation
git branch -d feature/normalisation        # delete local branch
git push origin --delete feature/normalisation  # delete remote branch
```

## Pull requests

A pull request (PR) is a proposal to merge one branch into another. It is also a conversation — collaborators can review, comment, request changes, and approve.

### Opening a PR

1. Push your branch to GitHub.
2. Go to the repository on GitHub — you will see a prompt to open a PR.
3. Write a clear title and description: what changed, why, and how to test it.
4. Request a reviewer.

### Reviewing a PR

- Use inline comments to discuss specific lines.
- Use the **Files changed** tab to see the diff.
- Approve when satisfied, or request changes with clear notes.

### After merge

Delete the branch on GitHub (there is a button after merging), then locally:

```bash
git checkout main
git pull
git branch -d feature/normalisation
```

## Keeping your branch up to date

If `main` has moved on while you were working on your branch, bring the changes in:

```bash
git checkout feature/normalisation
git fetch origin
git merge origin/main
# or:
git rebase origin/main
```

`merge` preserves the full history; `rebase` replays your commits on top of `main` for a cleaner linear history. Both are valid — pick one convention per team.

## Merge conflicts

A merge conflict happens when two branches edit the same part of a file differently.

```{warning}
Conflicts look scary but are normal. Git stops and asks you to decide — it never silently chooses for you.
```

### What a conflict looks like

```
<<<<<<< HEAD
result = normalise(data, method="z-score")
=======
result = normalise(data, method="minmax")
>>>>>>> feature/normalisation
```

- `<<<<<<< HEAD` — your current branch's version
- `=======` — the divider
- `>>>>>>> feature/normalisation` — the incoming branch's version

### Resolving a conflict

1. Open the file in your editor.
2. Decide which version to keep (or write a combination).
3. Delete all conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
4. Stage and commit:

```bash
git add analysis.py
git commit -m "Resolve merge conflict: use z-score normalisation"
```

### Tips for fewer conflicts

- Keep branches short-lived and focused on one thing.
- Pull from `main` frequently while working on a branch.
- Communicate with collaborators when touching the same file.

## Further reading

- [GitHub Flow guide](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Atlassian Git tutorials](https://www.atlassian.com/git/tutorials)
- [Oh Shit, Git!?!](https://ohshitgit.com/) — plain-language fixes for common mistakes
