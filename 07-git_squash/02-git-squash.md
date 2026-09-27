# Advanced: Autosquash with Fixup

## Step 1: Tag a fixup commit

    git commit --fixup <commit-hash>

## Step 2: Auto-squash

    git rebase -i --autosquash HEAD~N

Git reorders commits and marks them for squashing automatically.

# Real DevOps Use Case

You develop a feature across 6 commits:

- Add component
- Fix logic
- Rename variable
- Change structure
- Final cleanup
- Add test cases

Before merging to `develop`, squash into 1 commit:

    git rebase -i HEAD~6

Now, reviewers and release notes see 1 clean summary commit instead of messy mid-dev changes.

# Squashing & Shared Branches

Squashing rewrites history → NEVER squash commits on a shared branch unless coordinated.

If others have pulled the branch:

- Use `git push --force-with-lease` (safer than `--force`)
- Or, squash before pushing at all

# Why Use HEAD~N?

`HEAD~N` tells Git to go N commits back from the current commit. For example:

- `HEAD~3` = last 3 commits before HEAD
- `HEAD~1` = the parent of HEAD

You can also squash from a specific commit:

    git rebase -i <base-commit>

# Best Practices

| Tip | Reason |
|---|---|
| Squash before merging PRs | Clean history for release & review |
| Use `--autosquash` for fixups | Saves time in long branches |
| Don't squash shared history | Avoid breaking collaborators |
| Test after squashing | Conflicts can lead to broken merges |
| Add meaningful messages | Merged commit should reflect all the changes |

# Squash in Pull Requests

## GitHub

- Enable "Squash and merge" in PR options
- Auto-squash PR commits into one on merge

## GitLab

- Use "Squash commits when merging"
- Default per project

## Bitbucket

- Set PR merge strategy to squash in settings

# Git Squash Commands Summary

| Task | Command |
|---|---|
| Rebase interactively | `git rebase -i HEAD~N` |
| Mark commits for squash | Change `pick` → `squash` |
| Auto-squash | `git commit --fixup <hash>` + `git rebase -i --autosquash HEAD~N` |
| Merge with squash | `git merge --squash <branch>` |
| Force push after squash | `git push --force-with-lease` |
| Undo squash | Use `git reflog` and `git reset` |

# Real Team Story

At SHACKVERSE, developers were making 15–20 commits per feature, including:

- Code commits
- Fixes
- Console logs
- Refactors

Before each PR merge, they squash into a single commit. This helped:

- Simplify history
- Speed up code reviews
- Improve release changelogs
- Prevent build trigger overloads in CI

# Git Squash vs Git Revert

| Action | Git Squash | Git Revert |
|---|---|---|
| Combine commits | Yes | No |
| Keep history intact | No | Yes |
| Rewrites commit IDs | Yes | No |
| Safe for shared branches | No | Yes |
| Reverses changes | No | Yes |

# Avoid These Mistakes

- Squashing commits after push without team sync
- Leaving vague commit messages like "fix"
- Squashing bugfixes before they’re reviewed
- Forgetting to test after squash
- Forcing push without checking shared usage

# Suggested Practice

1. Create a feature branch
2. Make 5+ commits (WIP, fix, rename, etc.)
3. Squash interactively
4. Test output
5. Force-push to remote
6. Revert using reflog if you mess up

# Mental Model

Imagine writing a book:

- Every sentence is a commit
- Your feature branch is a draft
- Squashing is like writing the final chapter: clear, edited, no typos

You don’t publish every note — just the best version.
