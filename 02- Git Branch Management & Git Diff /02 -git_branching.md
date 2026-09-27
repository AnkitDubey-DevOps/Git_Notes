# Corporate Git Branching Strategies

There are multiple branching models used across companies. Let's break them down:

## 1. Git Flow (Vincent Driessen Model)

Most detailed and process-heavy strategy. Great for release-based projects.

### Branches Used

- `main` (production)
- `develop` (integration)
- `feature/*`
- `release/*`
- `hotfix/*`

### Flow

1. Start from `develop`, create `feature/x`
2. When done → merge back to `develop`
3. To release → create `release/1.0`
4. Final release → merge `release/1.0` into `main` and tag it
5. Hotfixes start from `main` and also go into `develop`

### Git Flow Example

    # Create feature
    git checkout develop
    git checkout -b feature/cart-page

    # Work, commit, then:
    git checkout develop
    git merge feature/cart-page

    # Prepare release
    git checkout -b release/v1.0

    # Tag and test...

    # Final release
    git checkout main
    git merge release/v1.0
    git tag v1.0

    # Corporate Git Branching Strategies
    # Bring back to develop

    git checkout develop
    git merge release/v1.0

## 2. GitHub Flow (Simpler, for Continuous Deployment)

Great for startups or SaaS.

### Branches

- `main` only (no `develop`)
- Feature branches → Pull Requests (PRs)

### Flow

1. Create feature branch from `main`
2. Open PR early for visibility
3. Merge after review/tests

### Command

    git checkout -b feature/signup-form main

## 3. Trunk-Based Development

Used by large-scale teams like Google or Facebook.

- Developers commit frequently to `main` or trunk
- Feature flags used to hide incomplete features
- Encourages continuous integration + testing

## When to Use What?

| Team Type | Strategy |
|---|---|
| Enterprise, releases, QA cycles | Git Flow |
| Startups, speed-focused | GitHub Flow |
| Large-scale CI/CD with feature flags | Trunk-Based |

# Best Practices for Branch Management

- Keep branch names meaningful (`feature/signup-form`)
- Delete stale branches regularly
- Avoid working too long in isolation (merge often)
- Rebase locally, merge remotely
- Use `git fetch --prune` to remove stale remote refs
- Use Pull Request templates to enforce code quality

# Troubleshooting Branching Issues

| Issue | Fix |
|---|---|
| Accidentally created branch from wrong base | `git rebase correct-base` |
| Wrong commits in branch | `git reset` or `git cherry-pick` |
| Merge conflict | Resolve manually → `git add` → `git commit` |
| Outdated remote refs | `git fetch --prune` |

# Hands-on Practice Ideas

1. Simulate a complete Git Flow cycle from `develop` to `main`
2. Introduce a merge conflict between branches and resolve it
3. Create and delete 5 dummy branches, both local and remote
4. Use GitHub/GitLab to create a PR from a branch

