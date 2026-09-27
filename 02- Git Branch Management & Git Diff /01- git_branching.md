# Part 1: Git Branching – Concepts, Syntax, Strategies, and Internals

## What is a Git Branch?

A branch in Git is simply a lightweight movable pointer to a specific commit. It's not a copy of files or directories — it’s a reference to a snapshot in your project’s history.

Think of a Git branch like a bookmark in the commit timeline. You can create a new one, move it, or merge it without duplicating code.

## How Git Branches Work Under the Hood

- Git maintains a special pointer called HEAD, which references your current branch.
- Each branch is a file in `.git/refs/heads/branch-name`, containing the SHA-1 hash of the latest commit.
- When you create a branch, Git copies the current commit hash into a new file in `.git/refs/heads/`.

### Example

    git branch feature-x
    cat .git/refs/heads/feature-x

You’ll see a commit hash like:

    3adf23d5d61e2e6aee43b00e93e93e1e9e32a012

This means feature-x is pointing to that commit. As you commit more, Git updates that pointer.

## Basic Branching Commands (with Scenarios)

| Task | Command | Scenario |
|---|---|---|
| Create branch | `git branch feature-x` | You want to work on a new login feature |
| Switch to branch | `git checkout feature-x` or `git switch feature-x` | Start working on the login feature |
| Create + switch | `git checkout -b feature-x` | Combine both actions |

## Basic Branching Commands (Continued)

| Task | Command | Scenario |
|---|---|---|
| List branches | `git branch` | See all local branches |
| Delete branch | `git branch -d feature-x` | After merge, cleanup |
| Force delete | `git branch -D feature-x` | When a branch isn't merged but you want to delete it anyway |
| Rename branch | `git branch -m old-name new-name` | Rename for clarity |
| See remote branches | `git branch -r` | Collaborators’ branches |
| See all branches | `git branch -a` | Local + remote branches |

## Advanced Branching Internals

When you commit in a new branch:

- Git writes a new commit object with a new SHA-1
- Updates the current branch pointer (e.g., `feature-x`) to this new hash
- The HEAD also moves with it since you’re on that branch

This allows non-linear development, where branches diverge from and merge back into mainline code.

## Small Assignment

    git init
    echo "Hello World" > hello.txt
    git add . && git commit -m "Initial commit"

    # Create branch
    git branch feature-login
    git checkout feature-login
    echo "Login Page" > login.html
    git add . && git commit -m "Add login page"

Then, go back to main and see that `login.html` doesn’t exist — that’s branch isolation.

# Git Branch Naming Conventions in Teams

Use clear, consistent names that reflect purpose and scope.

| Branch Type | Example | Purpose |
|---|---|---|
| main or master | - | Production-ready code |
| develop | - | Staging area for integration |
| feature/* | `feature/payment-integration` | New feature development |
| bugfix/* | `bugfix/login-crash` | Fix minor issues not in prod |
| hotfix/* | `hotfix/critical-vuln` | Patch production issues immediately |
| release/* | `release/v1.2.0` | Final stabilization before release |
