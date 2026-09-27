# What is Git Rebase?

## Definition

`git rebase` takes a branch and reapplies its commits on top of another branch.

    git rebase main

This rewrites the commit history by changing the parent of your commits, making it look like they were created after the latest commit on `main`.

## How Git Rebase Works Internally

1. Git finds the common ancestor between `feature-x` and `main`.
2. It temporarily removes all commits on `feature-x` after that point.
3. It reapplies each commit from `feature-x` on top of `main`, one by one.
4. Each commit gets a new SHA-1 hash (because history is rewritten).

## Visual Example (Before Rebase)

    A---B---C (MAIN)
     \
      D---E (FEATURE-X)

After `git checkout feature-x && git rebase main`:

        D'--E' (FEATURE-X)
       /
    A---B---C (MAIN)

- D and E are rewritten as D' and E'
- Looks like the feature was developed after C

## When to Use Git Rebase

| Situation | Why Use Rebase |
|---|---|
| Before merge | Clean history without merge commits |
| Solo developer | Rewriting own history is safe |

## When to Use Git Rebase

| Situation | Why Use Rebase |
|---|---|
| Local changes | Rebase before pushing |
| Linear project history | Easier to debug with `git bisect` |

# Pros of Git Rebase

- Keeps history linear and clean
- Easier to navigate with `git log`
- Avoids noisy merge commits
- Ideal before merging to `main`

# Cons of Git Rebase

- Rewrites history — dangerous if shared with others
- Conflicts can occur with each commit replayed
- Should not rebase public branches

# Golden Rule of Rebase

Never rebase shared/public branches. Only rebase local commits that haven’t been pushed.

# Git Rebase – Real World Demo

## Step-by-Step

    # 1. Create main commit
    git init
    echo "App start" > app.txt
    git add . && git commit -m "Initial commit"

    # 2. Create feature branch
    git checkout -b feature-ui
    echo "UI code" >> app.txt
    git add . && git commit -m "UI change 1"
    echo "UI fix" >> app.txt
        git add . && git commit -m "UI fix"

    # 3. Switch to main and make changes
    git checkout main
    echo "Main update" >> app.txt
    git add . && git commit -m "Main update"

    # 4. Rebase feature onto updated main
    git checkout feature-ui
    git rebase main

## If Conflicts Happen

    # Manually fix file
    git add file.txt
    git rebase --continue

## Rebase Abort

    git rebase --abort  # Roll back rebase if needed

# Merge vs Rebase: Battle of Titans

| Feature | Merge | Rebase |
|---|---|---|
| Commit History | Preserves full tree | Rewrites history |
| Merge Commit | Always adds one | No merge commit |
| Use In Teams | Preferred for shared code | Preferred for local branches |
| Rewriting History | No | Yes |
| Traceability | Better | Cleaner but less traceable |
| Conflict Resolution | Once | Multiple times (per commit) |

# Practical Scenario

Your teammate merges `feature-a` into `main`. You’re working on `feature-b`.

- To catch up:

    # Merge
    git checkout feature-b
    git merge main

Or:

    # Rebase
    git checkout feature-b
    git rebase main

Rebase gives a clean history; merge preserves context.

# Common Merge/Rebase Errors & Fixes

| Error | Cause | Fix |
|---|---|---|
| Merge conflict | Overlapping file edits | Fix file → `git add` → `git commit` |
| "Refusing to rebase" | Uncommitted changes | `git stash` or commit first |
| "No rebase in progress" | Wrong usage | Use `git rebase --abort` |
| Rebased wrong branch | Mistake in base branch | Use reflog: `git reflog`, then `git reset` |
| Accidentally rebased public branch | Broke team sync | Coordinate and force-push (use with care) |

# Pro Tips

- Use `git pull --rebase` to sync your branch linearly with upstream
- Enable auto-setup rebase:

    git config --global pull.rebase true

- Visualize history before/after with:

    git log --oneline --graph --all

  # Advanced Techniques

- Interactive Rebase:

    git rebase -i HEAD~5

  - You can squash, reorder, rename, or drop commits

- Squash with rebase:

    git rebase -i main

  - Use `pick` for first commit, `squash` for the rest

# Corporate DevOps Relevance

| Stage | Merge or Rebase? | Why? |
|---|---|---|
| Local development | Rebase | Clean commit history |
| Pull Request | Merge | Trackable contribution |
| Production release | Merge | CI/CD traceability |
| Bugfix hotpatch | Merge | Safety and visibility |
| Feature sync | Rebase | No merge noise |
