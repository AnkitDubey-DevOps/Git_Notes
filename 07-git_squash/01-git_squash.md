# Git Squash

## What is Git Squash?

Git Squash is the process of combining multiple commits into a single one to create a cleaner, more readable, and more meaningful commit history.

It’s most commonly used when:

- A feature branch has many tiny or WIP commits
- You want to merge that feature into `main` with one polished commit

Squashing is typically done using interactive rebase, not the merge command.

## Why Squash Matters in Teams

| Situation | Why Squash? |
|---|---|
| Feature development | Developers make many WIP commits. Squashing summarizes them. |
| Code reviews | A single clean commit is easier to review |
| PR history cleanup | Keep Git history readable and focused |
| CI/CD pipelines | Avoid triggering builds on each micro-commit |
| Open-source contributions | Maintainers prefer 1 commit per feature/fix |

## Real Example Before Squash

    commit c5e1b4a Add more spacing to form
    commit a6d9e87 Fix typo
    commit 3b1aa84 Working login page
    commit 8af2930 Create login page

All part of a single feature.

## After Squash

    commit 41a9b70 Add login page feature

# Internals: What Happens During a Squash?

Under the hood:

- Git takes a list of commits
- Applies them one by one in memory
- Rewrites history with only the first commit (message + content)
- Merges content of the other commits
- Deletes the rest

This is possible via interactive rebase, which rewrites history.

# Interactive Rebase for Squash

    git rebase -i HEAD~4

This opens your default editor (e.g., Vim, Nano, VS Code) with:

    pick 8af2930 Create login page
    pick 3b1aa84 Working login page
    pick a6d9e87 Fix typo
    pick c5e1b4a Add more spacing to form

Change to:

    pick 8af2930 Create login page
    squash 3b1aa84 Working login page
    squash a6d9e87 Fix typo
    squash c5e1b4a Add more spacing to form

Then:

- Save and close the editor
- Git will prompt you to edit the new commit message
- Finalize the message → save → done

# Step-by-Step Git Squash Demo

## Scenario

You're working on `feature/login` with 4 commits. You want to squash them before merging to `main`.

## Step-by-Step

    # 1. Start with fresh repo
    git init squash-demo
    cd squash-demo

    # 2. Create commits
    echo "login base" > login.js
    git add . && git commit -m "Create login page"

    echo "login WIP" >> login.js
    git commit -am "Working login page"

    echo "Fix typo" >> login.js
    git commit -am "Fix typo"

    echo "Spacing update" >> login.js
    git commit -am "Add more spacing to form"

    # 3. Squash last 4 commits
    git rebase -i HEAD~4

## Edit From

    pick a1
    pick a2
    pick a3
    pick a4

## To

    pick a1
    squash a2
    squash a3
    squash a4

## Final Commit Message Prompt

    # Combine commit messages
    Create login page
    # Working login page
    # Fix typo
    # Add more spacing to form

Clean it up as:

    Add login page feature with layout, fixes, and spacing updates

# Git Squash Variants

| Method | Command |
|---|---|
| Interactive Rebase | `git rebase -i HEAD~N` |
| Squash during Merge | `git merge --squash feature-branch` |
| Auto-squash with Fixup | `git rebase -i --autosquash` |
| CI squash in GitLab | Merge request → squash before merge |

# Git Merge with --squash

    git checkout main
    git merge --squash feature/login
    git commit -m "Add login feature"

Good for keeping commit history clean without rebasing the branch.
