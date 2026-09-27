# Git Cherry-pick

## What is git cherry-pick?

`git cherry-pick` is a Git command that allows you to apply a specific commit from one branch to another, without merging the full branch.

It’s like copying a cherry (commit) from one cake (branch) and placing it on another cake (branch) without transferring the whole cake.

## Syntax

    git cherry-pick <commit-hash>

You can also cherry-pick a range of commits, multiple commits, or even cherry-pick with options for conflict resolution, message editing, or no-commit:

    git cherry-pick A B C
    # Multiple commits

    git cherry-pick A^..B
    # Commit range

    git cherry-pick --no-commit <hash>
    # Apply changes without committing

    git cherry-pick --edit <hash>
    # Edit message before commit

    git cherry-pick --signoff <hash>
    # Add sign-off line

## How Cherry-pick Works Internally

Cherry-pick operates by:

1. Identifying the diff introduced by a specific commit.
2. Applying that diff to your current working tree.
3. Staging the result and creating a new commit with the same content but a new hash.

The commit history is not preserved. Git re-applies the change, not the commit.

# Use Cases for Cherry-pick

| Scenario | Why Use Cherry-pick |
|---|---|
| Apply a hotfix from `hotfix/` to `main` and `develop` | Share a fix without merging the full branch |
| Backport a feature from `main` to `release/1.2` | Deliver a selective feature to older versions |
| Pick a specific bug fix into QA | Isolate fixes without unnecessary changes |
| Move only relevant commits from `feature/` into `develop` | Avoid experimental commits |

# Scenario

You’re on the `hotfix/login-crash` branch. You fixed a production bug and now need to:

- Apply the fix to `main`
- Also apply the fix to `develop`

# Step-by-Step Setup

    # 1. Start with repo and main branch
    git init cherry-demo
    cd cherry-demo
    echo "Login system" > app.js
    git add . && git commit -m "Initial commit"

    # 2. Create develop and hotfix branches
    git checkout -b develop
    echo "Working dev version" >> app.js
    git commit -am "Dev enhancements"

    git checkout -b hotfix/login-crash main
    echo "Hotfix for login crash" >> app.js
    git commit -am "Fix: login crash on null token"

# View Commit History

    git log --oneline --graph --all

Sample:

    e12b987 (hotfix/login-crash) Fix: login crash on null token
    f74c8d3 (develop) Dev enhancements
    a7f45dc (main) Initial commit

# Cherry-pick the Fix onto Main

    git checkout main
    git cherry-pick e12b987

Now your `main` has the fix without touching anything else from `hotfix/login-crash`.

# Now Cherry-pick to Develop

    git checkout develop
    git cherry-pick e12b987

Now the hotfix is reflected in all relevant branches — clean, safe, and traceable.

# Inspecting Cherry-picked Commits

    git log --oneline

You’ll see:

- A new commit hash
- Same commit message
- History is linear, no merge commits

Git creates a new commit with the same patch but a different parent — it’s a fresh snapshot.

