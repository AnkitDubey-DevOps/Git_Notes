# What is Git Stash?

`git stash` is a powerful Git feature that lets you temporarily save (or "stash away") your uncommitted changes, so you can work on something else without losing progress.

Think of it like placing your work on a shelf for safekeeping while you clean your workspace or switch tasks.

# Why Do Developers Use Git Stash?

| Situation | Why Stash Helps |
|---|---|
| Mid-task switch | Need to quickly switch branches without committing half-done work |
| Code review | Clean up your working directory before reviewing someone else’s branch |
| Experimentation | Try a quick fix without affecting your ongoing task |
| Pulling latest changes | Stash changes, pull latest code, re-apply stashed changes on top |
| Avoiding unnecessary commits | Save changes temporarily without polluting history with incomplete commits |

# Git Stash Syntax and Core Commands

| Command | Description |
|---|---|
| `git stash` | Stash both staged and unstaged changes |
| `git stash save "msg"` | Save stash with a custom message |
| `git stash list` | View all saved stashes |
| `git stash show` | Show changes in most recent stash (summary) |
| `git stash show -p` | Show patch (diff) of stash |

# Git Stash Commands

| Command | Description |
|---|---|
| `git stash apply` | Apply most recent stash without deleting it |
| `git stash pop` | Apply most recent stash and remove it |
| `git stash drop stash@{0}` | Delete a specific stash |
| `git stash clear` | Delete all stashes |
| `git stash branch <branch>` | Create a new branch from a stash |

# Under the Hood: How Git Stash Works Internally

`git stash` doesn’t just save a “working copy” of files — it creates three commits in the background:

1. **Commit A:** Your changes in the working directory (unstaged + staged)
2. **Commit B:** The staged index (only what was added)
3. **Commit C:** A merge commit combining them, stored under `refs/stash`

This stash reference is just like a regular commit — you can use `git log` or `git show` to explore it.

    git log refs/stash

Git stash uses the same internal mechanisms as commits — which means it’s powerful, scriptable, and robust.

# Demo: Real-World Scenario

## Scenario

You’re working on a feature in `feature/cart` branch. Suddenly, you need to switch to `develop` to fix a bug reported by QA — but you don’t want to commit half-done code. What do you do?

## Step-by-Step Demo

    # 1. Create a repo and branch
    git init stash-demo
    cd stash-demo
    echo "Version 1" > app.js
    git add . && git commit -m "Initial commit"
    git checkout -b feature/cart

    # 2. Make changes (but don’t commit)
    echo "Cart logic WIP" >> app.js

    # 3. Stash the changes
    git stash save "WIP: cart logic implementation"

At this point, your working directory is clean, and you can switch branches:

    git checkout develop

## Restore Your Work

Once the bug is fixed, go back to your branch and restore your work:

    git checkout feature/cart
    git stash pop

Boom — your changes are back.

# Anatomy of a Stash Entry

    git stash list

Output:

    stash@{0}: WIP on feature/cart: 1a2b3c4 Initial commit

This means:

- `stash@{0}` is the latest stash
- It came from `feature/cart`
- `1a2b3c4` is the base commit

You can inspect it further:

    git stash show stash@{0}
    git stash show -p stash@{0}

# Stash Structure Internals

Each stash is stored under:

    .git/logs/refs/stash

And internally composed of:

- A commit of working directory
- A commit of staged changes
- A reference to the original HEAD

These are created using low-level plumbing like:

    git write-tree
    git commit-tree

# Stashing with Only Tracked or Untracked Files

| Goal | Command |
|---|---|
| Stash only tracked files | `git stash -k` or `--keep-index` |
| Include untracked files | `git stash -u` |
| Include ignored files too | `git stash -a` |

# Advanced Demo: Stash While Pulling Latest Code

    # 1. You’ve edited app.js but want to pull new changes from remote
    git stash
    git pull origin main
    git stash pop

This avoids:

- Merge conflicts with your incomplete changes
- Creating a “junk commit” just for syncing

# Stash Naming: Best Practices

Give your stashes clear names — avoid “WIP” spam in history.

    git stash save "cart: UI state logic cleanup"

Use hooks/scripts to auto-stash with branch names and timestamps if needed.

# Stash Application Techniques

## Apply but Keep

    git stash apply stash@{0}

This re-applies your stash without deleting it. Useful when you’re experimenting.

## Pop (Apply + Delete)

    git stash pop stash@{0}

Safe for one-time restoration.

# Conflict Handling During stash pop

If the stash conflicts with your current branch:

- Git will halt the pop operation
- You’ll need to manually resolve conflicts
- Then run:

    git add .
    git stash drop stash@{0}

 Tools That Work With Git Stash 
Tool 
Integration 
VS Code 
Shows stash entries in Git sidebar 
GitKraken Visual stash management 
SourceTree UI-based stash pop/apply/drop 
Git GUI 
Basic stash support 
Git Stash + Branch: Experimental Power 
Let’s say your stash becomes worth saving in a proper branch: 
git stash branch experimental-ui stash@{0} 
This does: 
1. Creates experimental-ui branch 
2. Checks it out 
3. Applies the stash 
4. Drops it from stash list 
This is perfect for rapid prototypes that turned into features. 
Git Aliases for Stashing 
Speed up your workflow: 
git config --global alias.ss "stash save" 
git config --global alias.sp "stash pop" 
git config --global alias.sl "stash list" 
Now run git ss "msg" instead of git stash save.

Summary: Stash Command Cheatsheet 
Action 
Command 
Save stash 
git stash or git stash save "msg" 
List stashes 
git stash list 
Show changes 
git stash show or -p 
Apply stash 
git stash apply stash@{0} 
Pop stash 
git stash pop stash@{0} 
Drop stash 
git stash drop stash@{0} 
Clear all 
git stash clear 
Branch from stash git stash branch branch-name 
Real-World DevOps Use Cases 
Situation 
Stash Strategy 
CI/CD pipeline failing during manual fix Stash uncommitted debug work 
Emergency hotfix needed 
Stash incomplete feature and switch 
Developer context switching 
Save WIP progress with message 
Rebuilding corrupted branches 
Use stash branch for temporary experiments 
Training new devs 
Use stash to simulate change recovery scenarios 
Corporate Best Practices 
• Never stash without a message 
• Avoid leaving stashes around forever 
• Regularly git stash clear or integrate into branches 
• Use stash branch for transitioning temp work into PRs 
• Educate your team to treat stashes like drafts, not garbage bins
