# What is Git?

Git is a Distributed Version Control System where every developer has:

- A full copy of the repository (`.git` directory)
- Complete history including all branches, commits, and tags
- Ability to work offline, merge changes, and push to a central server

Unlike older systems (CVS, SVN), Git doesn’t rely on a central repository. It gives full power and flexibility to every contributor.

## Centralized vs Distributed

| Feature | Centralized VCS | Git (DVCS) |
|---|---|---|
| Server dependency | Mandatory | Not required |
| Offline work | No | Yes |
| Performance | Slower | Faster |
| History stored | Server-only | Local |
| Collaboration | Limited branching | Unlimited branching, rebasing |

## Why Git Changed the Game

- Snapshots instead of diffs
- Immutable commit history
- Fast operations via local metadata
- Advanced branching + merging strategies
- SHA-1 content hashing (ensures data integrity)
- Efficient file storage using content deduplication

# Git Terminology – Technical Explanation

| Term | Purpose | Behind-the-scenes |
|---|---|---|
| `git init` | Creates `.git/` | Initializes metadata directories and refs |
| Working Directory | Where devs modify files | Tracked vs Untracked files |
| Staging Area | Area to prepare commits | `.git/index` file tracks staged file metadata |
| Commit | Saves current state | Stores a new object with SHA-1 and links |
| Push | Uploads commits to remote | Affects `.git/refs/heads/branch` remotely |
| HEAD | Pointer to the current branch | Contents of `.git/HEAD` |
| Refs | Branch, tag, and HEAD pointers | Stored in `.git/refs/` and `packed-refs` |

# Git’s Core Object Model

Every piece of data in Git is stored as an object in the `.git/objects` directory. These objects are:

| Object Type | Description |
|---|---|
| Blob | Stores file contents |
| Tree | Stores directory structure |
| Commit | Stores snapshot reference, metadata, and parent(s) |
| Tag | Reference to a commit or object |

# Blob Object

- Raw content of a file (no filename)
- Stored in a compressed format
- SHA-1 hash used as ID

```bash
echo "Hello World" > file.txt
git hash-object file.txt
This generates a blob object stored in .git/objects/ab/123456...```

