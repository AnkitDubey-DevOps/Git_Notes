# Git Revert

## What is `git revert`?

`git revert` is a safe, history-preserving command used to undo the effect of a previous commit by creating a new commit that reverses the changes introduced by the original one.

Unlike `git reset`, revert does not rewrite commit history — making it ideal for shared/public branches.

## Why Revert?

| Scenario | Why Use Revert |
|---|---|
| A deployed change broke production | Revert safely without removing history |
| A feature merged prematurely | Revert it from `main` while fixing it |
| Remove a bad commit without a force push | Shared branches remain stable |
| Legal/audit compliance | Retain traceability while fixing mistakes |

# How Git Revert Works Internally

- Git looks at the diff (changes) introduced by the target commit.
- Then it creates a new commit that reverses that diff.
- The new commit is added on top of the current branch.

Example:

    git revert <commit-hash>

This creates a new commit with a message like:

    Revert "Add new payment processor"

# `git revert` vs `git reset` vs `git checkout`

| Command | Behavior | Safe for Teams? |
|---|---|---|
| `git revert` | Adds a new commit to undo changes | Yes |
| `git reset` | Rewrites commit history | No (unsafe on shared branches) |
| `git checkout` (legacy) / `git restore` | Changes files in working directory | Yes |

# Git Revert: Basic Syntax

    git revert <commit>

## More Options

| Option | Use |
|---|---|
| `--no-commit` | Stage changes only, don't commit yet |
| `--edit` | Open editor to modify the commit message |
| `--mainline` | Used to revert merge commits (see below) |
| `-n` or `--no-commit` | Combine with multiple reverts before one commit |
| `--signoff` | Append signoff footer (used in audits or signed projects) |

# Demo: Git Revert in Action

## Scenario

You're in `main`, and a recent commit caused a bug in production. You want to undo it without touching previous work or rewriting history.

## Step-by-Step Demo

### 1. Initialize Repo

    git init revert-demo
    cd revert-demo
    echo "Initial content" > app.js
    git add . && git commit -m "Initial commit"

### 2. Add a Bad Change

    echo "console.log('Breaking change');" >> app.js
    git commit -am "Add breaking log"

### 3. Get Commit History

    git log --oneline

You'll see something like:

    c3d5a1a Add breaking log
    0a7b3c2 Initial commit

### 4. Revert the Bad Commit

    git revert c3d5a1a

Git opens your default editor:

    Revert "Add breaking log"

    This reverts commit c3d5a1a.

You can edit or accept and save.

Git then creates:

    [main 8b31a73] Revert "Add breaking log"

Done. History is intact, code is fixed.

# What Happens Under the Hood

Git internally computes:

    diff <parent-of-c3d5a1a> c3d5a1a

Then it reverses that diff and applies it to the current working directory.

A new commit is created with reversed changes.

# Resulting Commit History

    git log --oneline

Output:

    8b31a73 Revert "Add breaking log"
    c3d5a1a Add breaking log
    0a7b3c2 Initial commit

The original "bad" commit is still in the history, but now reversed by a new commit.

# Revert Without Committing Immediately

Use this if you want to modify or combine multiple reverts:

    git revert --no-commit c3d5a1a

    # Make changes, then:
    git commit -m "Revert bad change and update logic"

# Reverting Merge Commits (Advanced)

Git cannot revert a merge commit without extra info.

    git revert -m 1 <merge-commit-hash>

Here:

- `-m 1` specifies the mainline parent (usually the base branch like `main`)
- Needed because merge commits have multiple parents

> Dangerous if misunderstood — recommended only for advanced users.

# Use Cases by Role

| Role | Use Case |
|---|---|
| Developer | Undo a bad patch or experimental commit |
| DevOps Engineer | Roll back a CI/CD-deployed change |
| QA Engineer | Remove buggy code added by accident |
| Project Manager | Undo PR merge while keeping traceability |
| Open-source Contributor | Clean up public history without rebasing |

# Multi-Commit Revert (Sequential)

    git revert commit1 commit2 commit3

Git will revert them in order and pause on each if conflicts occur.

To batch and commit together:

    git revert --no-commit commit1 commit2
    git commit -m "Revert feature A and B"

# Revert in GUI Tools

| Tool | Behavior |
|---|---|
| VS Code | Right-click commit → Revert |
| GitHub | On PR → Click "Revert" button |
| GitLab | Same as above |
| GitKraken | Click commit → Revert |
| IntelliJ | Commit window → right-click → Revert |

# Common Revert Errors & Fixes

| Error | Cause | Fix |
|---|---|---|
| "Your local changes would be overwritten" | Uncommitted changes | Stash or commit them first |
| "Reverting a merge needs mainline specified" | Trying to revert a merge | Use `-m 1` with caution |
| "Conflict during revert" | Overlapping changes | Manually resolve, then `git revert --continue` |
| Accidentally reverted wrong commit | Wrong hash used | Use `git reflog` to find previous `HEAD` and reset |

# Revert vs Reset vs Rebase

| Feature | Revert | Reset | Rebase |
|---|---|---|---|
| Safe for team use | Yes | No | No (on shared branches) |
| History rewrite | No | Yes | Yes |
| Commit added | Yes | No | Depends |
| Use case | Undo public commit safely | Remove local commits | Reorder & rewrite local commits |
| Example | Rollback prod | Cleanup WIP | Linearize PR |

# Git Revert in Production

- Used to quickly fix issues without downtime
- GitOps platforms like ArgoCD or FluxCD can monitor Git → Deploy rollback commit automatically
- Maintains full audit trail

# Git Aliases for Revert Workflow

    git config --global alias.undo 'revert HEAD'
    git config --global alias.rlog 'log --grep=Revert'

Now run:

    git undo     # Reverts the latest commit
    git rlog     # Shows all revert commits

# Git Revert Cheatsheet

| Action | Command |
|---|---|
| Revert single commit | `git revert <hash>` |
| Revert with no commit | `git revert --no-commit <hash>` |
| Revert merge commit | `git revert -m 1 <merge-hash>` |
| View all reverts | `git log --grep=Revert` |
| Undo a revert | `git revert <revert-hash>` (re-reverts) |
| Abort revert | `git revert --abort` |

# Practice Ideas

1. Create a repo, make 5 commits
2. Revert the 3rd commit
3. Revert a range of commits
4. Revert a revert (double revert)
5. Revert a merge (advanced)
6. Check diffs between original and revert

# How Git Revert Affects History Graph

## Before

    A --- B --- C (MAIN)

## After Reverting B

    A --- B --- C --- D (MAIN)
         \
          (REVERT OF B)

Git adds a new commit; B is still part of history but its changes are undone in D.

# Revert a Revert: "Undo the Undo"

    git revert <revert-commit-hash>

Git sees the previous revert and re-applies the original changes.

Useful when you revert something, then realize it wasn't broken after all.

# Real-World Use Case: SHACKVERSE Team

- Developer merges an experimental feature to `main` by mistake.
- CI/CD deploys it → production breaks.
- DevOps uses:

      git revert <feature-commit>

- Rollback happens in Git.
- ArgoCD redeploys the fixed version.
- Postmortem includes the revert ID for traceability.

# Final Thoughts

Git Revert is the safest, cleanest way to undo changes on a shared branch.

- Safe in production
- Maintains team trust
- Keeps history readable
- Works with CI/CD
