# Git Tags

## What is a Git Tag?

A Git tag is a label or marker attached to a specific commit in your repository. It’s used to mark important points in history — usually for releases, milestones, or production deployments.

Unlike branches, tags are immutable — they don’t move. Once a tag is created, it always points to the same commit unless manually deleted or re-created.

## Why Tags Matter in Software Development

- Semantic Versioning (SemVer): `v1.0.0`, `v2.3.4`
- Snapshot releases for QA or Production
- Deployment rollbacks or reproducibility
- Dependency tracking (e.g., Python, Go modules, Docker)
- CI/CD triggers
- Audit & compliance checkpoints

## Types of Git Tags

| Tag Type | Description | Use Case |
|---|---|---|
| Lightweight | Just a name pointing to a commit (like a branch, no metadata) | Quick personal markers |
| Annotated | A full Git object with metadata (author, message, timestamp, GPG signature) | Public releases, production tags |

## 1. Lightweight Tags

    git tag v1.0

- Does not store metadata
- Just a direct reference to the commit hash
- Equivalent to:

    git tag -l
    git show v1.0

## 2. Annotated Tags

    git tag -a v1.1 -m "Release v1.1"

- Creates a new tag object stored in `.git/objects/`
- Includes:
  - Tag message
  - Tagger (author)
  - Date
  - GPG signature (if configured)
- Better for public releases and audits

You can verify:

    git show v1.1

## Tag Creation – Full Syntax Reference

| Action | Command |
|---|---|
| Create lightweight tag | `git tag v1.0` |
| Create annotated tag | `git tag -a v1.1 -m "Release v1.1"` |
| Tag a specific commit | `git tag -a v1.2 <commit-hash>` |
| Sign a tag (GPG) | `git tag -s v1.3 -m "Signed release"` |
| List all tags | `git tag` |
| Search tags | `git tag -l "v1.*"` |
| Show tag content | `git show v1.1` |
| Delete tag locally | `git tag -d v1.0` |
| Delete tag remotely | `git push origin --delete v1.0` |
| Push tags to remote | `git push origin --tags` |
| Push single tag | `git push origin v1.2` |

# Git Tag Demo – Step-by-Step Walkthrough

## Step 1: Setup

    git init tag-demo
    cd tag-demo
    echo "Initial app version" > app.js
    git add . && git commit -m "Initial commit"

## Step 2: Tag the Commit

    git tag -a v1.0 -m "First official release"

## Step 3: Simulate New Features

    echo "Feature A" >> app.js
    git commit -am "Add feature A"

    echo "Feature B" >> app.js
    git commit -am "Add feature B"

## Step 4: Add New Tag

    git tag -a v1.1 -m "Second release with feature A & B"

## Step 5: Push to Remote

    git remote add origin <your-repo-url>
    git push origin --tags

# Behind the Scenes: How Tags Work Internally

Git stores tags inside `.git/refs/tags/` (like branches are in `.git/refs/heads/`).

Each tag file contains the SHA-1 hash of the commit (or tag object in case of annotated).

You can inspect the object:

    git cat-file -p <tag-hash>

## Annotated Tag Structure

- Tag object references the commit object
- Stored in `objects/`
- Contains:

    object <commit-hash>
    type commit
    tag v1.0
    tagger John Doe <john@company.com> ...
    message

# Signed Tags for Security

    git tag -s v1.2 -m "Signed tag"

Requires GPG setup:

    gpg --list-secret-keys
    git config --global user.signingkey <key>

Verify a signed tag:

    git tag -v v1.2

Used in high-compliance environments for traceability and authenticity.

# Tagging Older Commits

Use Git log to find the hash:

    git log --oneline

Then:

    git tag -a v0.9 <commit-hash> -m "Pre-release"

# Naming Convention Best Practices

| Tag Type | Format |
|---|---|
| Releases | `v1.0.0`, `v2.3.1` |
| Milestones | `mvp-1`, `rc-2` |
| Hotfix | `hotfix-2024-04-10` |
| Signed tags | `signed-v1.0` |

Follow Semantic Versioning:

- MAJOR.MINOR.PATCH = Breaking, Feature, Fix
- Example: `v2.1.4`

# Tags in CI/CD Pipelines

| CI/CD Use Case | How Tags Help |
|---|---|
| Trigger deploys | e.g., only deploy on new `v*` tag |
| Rollbacks | Easily revert to `v1.3` tag |
| Docker builds | Use tags as `docker build -t myapp:v1.0 .` |
| Artifact versioning | Save JARs, ZIPs, etc. as `release-v1.0.zip` |
| Immutable builds | Tags guarantee identical code for all environments |

# Practical Real-World Use Cases

| Role | Use Case |
|---|---|
| Developer | Marks final build before sending to QA |
| QA | Tags builds as `tested-v1.2` |
| DevOps | Tags production-ready builds for CI/CD |
| Release Manager | Uses signed annotated tags for release notes |
| Project Manager | Reviews milestone delivery via tags |

# Undo or Manage Tags

| Task | Command |
|---|---|
| Delete locally | `git tag -d v1.0` |
| Delete remote | `git push origin --delete v1.0` |
| Update tag (delete + re-create) | Delete, then `git tag -f v1.0 <hash>` |
| Clone repo with tags | `git clone --branch v1.0 <url>` |

# Gotchas & Safety Tips

- Tags are not pushed automatically!
- You must explicitly `git push origin --tags`.
- Avoid re-tagging history: This can confuse CI/CD, cause production mishaps
- Do not move annotated tags on public repos
- Protect release tags using GitHub/GitLab tag protection rules

# Deep Technical Exercise

    # Tag current commit
    git tag -a v2.0 -m "Major release"

    # View objects
    git rev-parse v2.0
    git cat-file -p <tag-hash>

Observe:

- Tag object
- Commit object it points to
- Differences from lightweight tag

# Tags + Branch Strategy

| Task | Strategy |
|---|---|
| Finalize release | Tag `release/x` → merge to `main` |
| Hotfix production | Tag hotfix commit for rollback |
| Archive old builds | Tag and archive `v0.9` |
| Continuous delivery | Use tags for semver release notes |

# Tag Management Cheatsheet

| Action | Command |
|---|---|
| Create lightweight | `git tag v1.0` |
| Create annotated | `git tag -a v1.1 -m "msg"` |
| Sign tag | `git tag -s v1.2 -m "msg"` |
| List all | `git tag` |
| Show tag info | `git show v1.1` |
| Push tags | `git push origin --tags` |
| Push one tag | `git push origin v1.1` |
| Delete local | `git tag -d v1.0` |
| Delete remote | `git push origin --delete v1.0` |
| Tag old commit | `git tag -a v0.9 <hash>` |

# Real Team Stories

- Startup Release Flow: Tags trigger GitHub Actions that deploy Docker containers to Kubernetes.
- Enterprise App: Signed tags required by infosec team for every release.
- Monorepo Setup: Each team tags their microservice inside the same repo.
- Banking App: Tags mapped to regulatory testing environments.

# Final Thoughts

Git tags are simple to use but incredibly powerful when used strategically. They enable:

- Release tracking
- Rollback safety
- Semantic versioning
- Deployment automation
- Immutable history
