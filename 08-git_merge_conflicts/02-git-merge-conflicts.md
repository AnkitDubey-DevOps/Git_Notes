# View History of Conflicts

You can see the commits involved in the conflict:

    git log --merge

This helps you understand why the conflict occurred.

## Conflict Recovery & Undo

| Action | Command |
|---|---|
| Abort merge | `git merge --abort` |
| Abort rebase | `git rebase --abort` |
| Restart merge | Delete `MERGE_HEAD` |
| Undo changes | `git reset --hard` |
| Go back | `git reflog` + `git reset <old-hash>` |

## Strategies to Avoid Merge Conflicts

| Practice | Benefit |
|---|---|
| Pull before working | Start from latest code |
| Communicate with team | Coordinate shared files |
| Use smaller branches | Easier merges |
| Avoid long-lived branches | Less divergence |
| Prefer feature toggles over massive changes | Incremental updates reduce conflicts |
| Use `git blame` to understand who changed what | Helps during resolution |

# Advanced Conflict Scenarios

## 1. Binary Files

Git cannot auto-merge binary files.

Use ours or theirs strategy:

    git checkout --ours file.pdf
    git add file.pdf

## 2. Line Endings (Windows vs Unix)

Standardize with `.gitattributes`:

    *.js text eol=lf

## 3. White Space Differences

Avoid unnecessary conflicts by ignoring whitespace:

    git diff --ignore-space-change

    git merge -Xignore-space-change branch

# Conflict Frequency by Merge Strategy

| Strategy | Conflict Likelihood |
|---|---|
| Rebase | High (commits replayed one by one) |
| Merge | Medium (1-time conflict resolution) |
| Cherry-pick | Medium (depends on overlap) |
| Squash Merge | Low (single commit applied) |
| Trunk-based Dev | Low (short-lived branches) |

# CI/CD Merge Conflict Handling

- CI checks PR mergeability
- Rebase + squash in merge strategy
- Auto-fail on unresolved conflicts
- Auto-close stale PRs to reduce risk
- GitHub/GitLab auto-block PRs that can't merge cleanly into the base branch.

# Aliases to Speed Up Conflict Workflows

    git config --global alias.conflict "diff --name-only --diff-filter=U"
    git config --global alias.resolve "add -u && commit -m 'Resolve conflicts'"

Then:

    git conflict
    git resolve

# Merge Conflict Resolution in UI Tools

| Tool | How |
|---|---|
| VS Code | Shows inline conflict resolution options ("Accept Incoming", etc.) |
| GitHub | Web-based conflict editor in PR |
| GitLab | Merge conflict editor |
| SourceTree | Side-by-side diff viewer |
| IntelliJ | 3-way merge with visual diff |

# Merge Conflict Command Recap

| Task | Command |
|---|---|
| Merge branches | `git merge branch-name` |
| View conflicts | `git status` |
| View conflict history | `git log --merge` |
| Resolve manually | Edit files, remove markers |
| Use mergetool | `git mergetool` |
| Abort merge | `git merge --abort` |
| Add resolved file | `git add <file>` |
| Complete merge | `git commit` |
