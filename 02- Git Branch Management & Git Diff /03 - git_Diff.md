# Part 2: Git Diff (Deep Dive)

## What is git diff?

`git diff` shows the line-by-line changes between two versions of a file or branch. It compares:

- Working Directory ↔ Staging Area
- Staging Area ↔ Last Commit
- One commit ↔ Another
- One branch ↔ Another

## Git Diff Basics

| Command | Compares | Use Case |
|---|---|---|
| `git diff` | Working dir ↔ Staging | See unstaged changes |
| `git diff --cached` | Staging ↔ Last commit | See what’s ready to commit |
| `git diff HEAD` | Working dir ↔ Last commit | See all uncommitted changes |
| `git diff A B` | Commit A ↔ Commit B | Compare two commits |
| `git diff branch1..branch2` | branch1 ↔ branch2 | See difference between branches |

# Practical Examples

    # Before staging
    git diff

    # After staging
    git diff --cached

    # Between two commits
    git diff 4e9f5d1 29c3dd2

    # Between branches
    git diff develop..feature-xyz

# Git Diff Output Anatomy

    diff --git a/index.html b/index.html
    index 83db48f..bf2692e 100644
    --- a/index.html
    +++ b/index.html
    @@ -5,7 +5,7 @@
    - # Hello World
    + # Welcome to Git Course

| Symbol | Meaning |
|---|---|
| `---` / `+++` | File names before/after change |
| `@@` | Line number ranges |
| `-` | Removed line |
| `+` | Added line |

# Advanced Diff Usage

- Compare last 3 commits:

      git diff HEAD~3 HEAD

- Compare staged vs HEAD:

      git diff --cached

- See changes word-by-word:

      git diff --word-diff

- See stat summary:

      git diff --stat
