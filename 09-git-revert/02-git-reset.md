# What is `git reset`?

`git reset` is a powerful Git command used to move the `HEAD` pointer and branch reference to a new commit, potentially modifying the staging area (index) and working directory depending on the mode.

Depending on how you use it, `git reset` can either be harmless or dangerously destructive.

# Three Core Components of Git State

To fully understand `git reset`, you must understand the three main states Git manages:

| Component | Description |
|---|---|
| `HEAD` | Points to the current commit |
| Index (Staging Area) | Files added with `git add` |
| Working Directory | Actual files on disk |

# Git Reset Modes Explained

| Mode | Command | What It Does |
|---|---|---|
| Soft | `git reset --soft HEAD~1` | Moves `HEAD`, keeps staging and working directory intact |
| Mixed (default) | `git reset --mixed HEAD~1` | Moves `HEAD`, resets staging area, working directory untouched |
| Hard | `git reset --hard HEAD~1` | Moves `HEAD`, resets staging area, and working directory |

# Under the Hood: What Git Reset Actually Does

- Git moves the `HEAD` pointer to a different commit (usually a previous one).
- Then, based on the mode:
  - **Soft:** Index and working tree are untouched.
  - **Mixed:** Index is reset to match `HEAD`; working directory is untouched.
  - **Hard:** Everything (`HEAD`, index, working tree) is reset.

# Visual Representation

## Before Reset

    HEAD -> C
    Index: matches C
    Working Dir: matches C

## After `--soft HEAD~1`

    HEAD -> B
    Index: matches C
    Working Dir: matches C

## After `--mixed HEAD~1`

    HEAD -> B
    Index: matches B
    Working Dir: matches C

## After `--hard HEAD~1`

    HEAD -> B
    Index: matches B
    Working Dir: matches B

# Git Reset Use Cases

| Use Case | Best Mode |
|---|---|
| Undo latest commit but keep changes staged | Soft |
| Unstage a file accidentally added | Mixed |
| Rollback working directory and commits completely | Hard |
| Rewind to clean state before a bad rebase | Hard |
| Remove sensitive data from recent commit | Hard (after removing file and rewriting history) |

# Git Reset: Demo Setup

## Step-by-Step

    git init reset-demo
    cd reset-demo
    echo "Line 1" > file.txt
    git add . && git commit -m "Initial commit"

Make 2 more commits:

    echo "Line 2" >> file.txt
    git add . && git commit -am "Add line 2"

    echo "Line 3" >> file.txt
    git add . && git commit -am "Add line 3"

Now check history:

    git log --oneline

You'll see:

    c3d Add line 3
    b2a Add line 2
    a1b Initial commit

# 1. Soft Reset Demo

    git reset --soft HEAD~1

## Effect

- `HEAD` now points to `Add line 2`
- Index still contains changes from `Add line 3`
- `git status`: changes ready to commit

    git status

Output:

    # On branch main
    # Changes to be committed:
    #   modified: file.txt

Perfect if you want to reword or amend the last commit:

    git commit -m "Add line 3 with corrections"

# 2. Mixed Reset Demo (Default)

    git reset --mixed HEAD~1

Or just:

    git reset HEAD~1

## Effect

- `HEAD` moves back one commit
- Index is reset
- Working directory still has changes

    git status

Output:

    # Changes not staged for commit:
    #   modified: file.txt

Best for unstaging files:

    git add file.txt

Re-stage selectively as needed.

# 3. Hard Reset Demo

    git reset --hard HEAD~1

## Effect

- `HEAD` moves back one commit
- Index and working directory are both reset
- The commit and changes from `Add line 3` are gone

    git log --oneline

The commit `Add line 3` is gone.

    cat file.txt

Only:

    Line 1
    Line 2

> **Danger!** You've lost both the commit and its changes unless backed up.

# Safety Precautions with Git Reset

| Safety Tip | Reason |
|---|---|
| Use `git reflog` | You can recover lost commits |
| Always stash or commit before hard reset | Prevent data loss |
| Never reset `--hard` on shared branches | May overwrite others' work |
| Use `--soft` when amending commit messages | Keeps your code safe |

# Reflog: Recovering After Mistakes

    git reflog

Shows history of `HEAD` movements:

    6e9 HEAD@{0}: reset: moving to HEAD~1
    c3d HEAD@{1}: commit: Add line 3

Recover lost commit:

    git checkout c3d

Or:

    git reset --hard c3d

# Reset a Specific File (Not Entire Commit)

    git reset HEAD file.txt

- Removes `file.txt` from staging area
- Keeps changes in working directory

Useful when you added a file by mistake and want to unstage without losing it.

# Git Reset vs Revert vs Checkout

| Feature | Reset | Revert | Checkout |
|---|---|---|---|
| Rewrite history | Yes | No | No |
| Safe for teams (on shared branches) | No | Yes | Yes |
| Affects commit history | Yes | Yes (non-destructive) | No |
| Undo last commit | Yes | Yes | No |
| Restore file | Yes | Yes | Yes |

# Advanced Reset Scenarios

## Reset to Specific Commit

    git reset --mixed <commit-hash>

Move your `HEAD` to an earlier commit by SHA.

## Clean Up WIP Commits

    git reset --soft HEAD~3
    git commit -m "Refactor: consolidated changes"

Useful before opening a Pull Request.

## Partial Hard Reset

    git checkout HEAD file.txt

Just reset one file to its last committed state.

Does not touch other files.

# Real Use Cases from DevOps

| Role | Use Case |
|---|---|
| Developer | Reset `HEAD~1` after accidental commit |
| DevOps Engineer | Hard reset cloned repo to clean state |
| QA Engineer | Reset feature branch before retesting |
| Release Manager | Roll back to known commit before tagging |
| GitOps Pipeline | Auto-reset state before re-applying configuration |

# Common Git Reset Mistakes

| Mistake | Fix |
|---|---|
| Used `--hard` on wrong branch | Use reflog to find lost commit |
| Staged wrong files and committed | Use `--soft`, re-stage |
| Hard reset shared branch | Use `git revert` instead |
| Lost important work | Check `.git/lost-found` or reflog |

# Git Reset + Git Reflog = Recovery Superpowers

## 1. Make a Bad Reset

    git reset --hard HEAD~2

## 2. Realize Mistake

    git reflog

## 3. Recover

    git reset --hard <previous-head-hash>

# Git Reset Command Summary

| Task | Command |
|---|---|
| Undo last commit, keep staged | `git reset --soft HEAD~1` |
| Undo last commit, keep code only | `git reset --mixed HEAD~1` |
| Wipe commit + code | `git reset --hard HEAD~1` |
| Reset specific file | `git reset HEAD file.js` |
| Recover from reset | `git reflog` + `git reset` |

# Real World Example

A XYZ engineer mistakenly committed an environment secret file.

Instead of rewriting public history, the engineer:

1. Created a new commit removing the file.
2. Reset his local branch:

       git reset --soft HEAD~1

3. Squashed everything into one clean commit:

       git commit -m "Remove secret file + cleanup"

# Final Thoughts

Git Reset is one of the most powerful and misunderstood Git commands.

- Use it to fix mistakes
- Use `--soft` for clean commit history
- Use `--mixed` to unstage
- Use `--hard` with backups or in safe branches

When used properly, it will supercharge your Git workflow and give you confidence to manage any branch state.
