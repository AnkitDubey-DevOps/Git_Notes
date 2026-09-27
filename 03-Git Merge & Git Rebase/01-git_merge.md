# What is Git Merge?

## Definition

`git merge` combines two branches by creating a new commit that brings together the histories of both branches.

    git merge feature-x

This merges `feature-x` into the current branch (usually `main` or `develop`) and preserves the commit history of both branches.

## How Git Merge Works Internally

1. Git finds the common ancestor of the two branches (called the merge base).
2. It performs a three-way merge between:
   - Current branch (e.g., `main`)
   - Target branch (e.g., `feature-x`)
   - The merge base
3. A new merge commit is created with two parent commits.

Merge commits are special because they have multiple parents. This is what enables non-linear history.

# Visual Example

    A---B---C (MAIN)
     \
      D---E (FEATURE-X)

After `git merge feature-x`:

    A---B---C---------F (MAIN)
     \             /
      D---E-------/  (FEATURE-X)

- F is the new merge commit
- History from both branches is preserved

# When to Use Git Merge

| Situation | Why Use Merge |
|---|---|
| Large teams | Keeps traceability of feature work |
| Long-lived branches | Better to show feature timelines |
| Finalizing features | Easy to visualize contribution via PRs |
| CI/CD pipelines | Merges trigger builds, not rebases |

# Pros of Git Merge

- Easy to understand
- Preserves complete history
- Safe for production merges
- Great for team collaboration (PRs)

# Cons of Git Merge

- Can create messy history with too many branches and merge commits
- Merge conflicts must be resolved manually
- Merge commits can be noisy for simple changes

# Git Merge – Real World Demo

## Step-by-Step

    # 1. Initialize repo
    git init
    echo "Hello" > file.txt
    git add . && git commit -m "Initial commit"

    # 2. Create feature branch
    git checkout -b feature-login
    echo "Login Feature" >> file.txt
    git add . && git commit -m "Add login feature"

    # 3. Go back to main
    git checkout main
    echo "Main Change" >> file.txt
    git add . && git commit -m "Main branch update"

    # 4. Merge feature
    git merge feature-login

If there are conflicts, Git will pause the merge until resolved.

## If Conflict Occurs

    # Resolve file.txt manually
    git add file.txt
    git commit
