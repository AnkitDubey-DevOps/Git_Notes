# Git Merge Conflicts

## What is a Merge Conflict?

A merge conflict in Git occurs when Git can’t automatically determine which version of a file to keep during a merge, rebase, or cherry-pick operation.

The most common scenario:

Both branches edited the same line of the same file differently.

## Typical Operations That Can Trigger Conflicts

| Operation | Description |
|---|---|
| `git merge` | Merging two branches with conflicting edits |
| `git rebase` | Replaying commits over another branch |
| `git cherry-pick` | Applying a commit that touches the same lines as existing code |
| `git pull` | Pulling remote changes when you have local edits |
| `git stash pop` | Applying stashed changes on top of edited files |

# Anatomy of a Merge Conflict

Let’s break this down visually.

Suppose we have:

### main branch

    console.log("User logged in");

### feature/login-enhancement branch

    console.log("Login successful");

Now if we try to merge `feature/login-enhancement` into `main`, Git cannot decide:

- Which line should be kept?
- Are they meant to be merged?
- Is one newer or more important?

# When Git Triggers Conflict

Git identifies that the same part of a file was modified differently in both branches. Since Git is not semantic-aware (it doesn’t understand programming logic), it flags the conflict and asks you to resolve it manually.

# Conflict Markers in the File

Git updates the conflicting file like this:

    <<<<<<< HEAD
    console.log("User logged in");
    =======
    console.log("Login successful");
    >>>>>>> feature/login-enhancement

### Markers

- `<<<<<<< HEAD` → Your branch (current)
- `=======` → Separator
- `>>>>>>> branch-name` → Incoming branch (to be merged)

Your job is to edit the file and remove the conflict markers, choosing (or combining) the right code.

# Merge Conflict Demo – Step-by-Step

## Scenario

You are on `main`. You and a teammate made conflicting edits on the same file.

## Step-by-Step Conflict Demo

    # 1. Create repo
    git init conflict-demo
    cd conflict-demo
    echo "console.log('Init');" > app.js
    git add . && git commit -m "Initial commit"

    # 2. Create new branch and edit
    git checkout -b feature-logging
    echo "console.log('User logged in');" > app.js
    git commit -am "Add login log"

    # 3. Go back to main and make conflicting change
    git checkout main
    echo "console.log('Login successful');" > app.js
    git commit -am "Change login message"

    # 4. Merge feature-logging into main
    git merge feature-logging

Git will show:

    Auto-merging app.js
    CONFLICT (content): Merge conflict in app.js
    Automatic merge failed; fix conflicts and then commit the result.

# Check Conflict Status

    git status

You’ll see:

    both modified: app.js

# Resolving Conflicts Manually

Open `app.js`, you’ll see:

    <<<<<<< HEAD
    console.log("Login successful");
    =======
    console.log("User logged in");
    >>>>>>> feature-logging

Edit to:

    console.log("User logged in successfully");

Then:

    git add app.js
    git commit -m "Merge feature-logging with conflict resolved"

Done.

# Tools to Resolve Conflicts

| Tool | Command |
|---|---|
| VS Code Merge Tool | Built-in UI |
| Git mergetool (CLI) | `git mergetool` |
| Meld | `git config --global merge.tool meld` |
| KDiff3 | GUI |
| SourceTree / GitKraken | Click-based conflict resolution |
| IntelliJ/GitHub Desktop | Visual merge helpers |

## Example: git mergetool

    git mergetool

It will launch the configured merge tool for each conflict.

# Tips for Clean Conflict Resolution

| Tip | Reason |
|---|---|
| Edit the file carefully | Never leave `<<<<<<<` or `=======` markers |
| Test after resolving | Conflicts can introduce bugs |
| Use `git diff` to verify changes | Ensure only expected differences |
| Use `git log --merge` | View conflicting commits side-by-side |
