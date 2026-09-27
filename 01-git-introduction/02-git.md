# Tree Object

- Maps directory names to blob/tree hashes
- Stores metadata (mode, type, name)

Use:

    git ls-tree HEAD

This lists the contents of the current commit tree.

# Commit Object

- Links to a tree object
- Stores author, committer, date, message
- Links to parent commit(s)

    git cat-file -p HEAD

Sample output:

    tree 48ab23...
    parent a1c9e7...
    author Aditya <adi@devops.com>
    date   Tue Apr 9 20:31:54 2024 +0530

    Initial commit

# Git Commit DAG (Directed Acyclic Graph)

Git doesn’t use a linear history — it uses a DAG of commits:

        A --- B --- C --- D  (main)
               \
                E --- F      (feature)

- Each commit points to its parent(s)
- Merges have multiple parents
- The DAG ensures consistency, no circular references

Git resolves branches and history using these relationships.

# SHA-1 Hashing

- Git uses SHA-1 (e.g., `9fceb02c...`) to uniquely identify:
  - Blobs
  - Trees
  - Commits
  - Tags

This makes Git secure and content-addressable.

Even a single character change produces a new hash.

# What Happens During Git Commands

## git add file.txt

- File is added to staging area
- Its content is hashed
- Blob is created (if not already exists)
- Index file is updated

## git commit -m "msg"

- Git creates:
  - A tree object for the directory
  - A commit object pointing to the tree and the parent commit
- Moves HEAD to this new commit

## git push

- Transfers commits and refs to remote repo via HTTP/SSH

# Inside the .git Directory

    .git/
    ├── HEAD
    ├── config
    ├── index
    ├── objects/
    ├── refs/
    │   ├── heads/
    │   └── tags/

- `HEAD` — Pointer to current branch
- `config` — Repo configuration
- `index` — Staging area (binary file)
- `objects/` — All Git objects (blobs, trees, commits, tags)
- `refs/` — Branch and tag pointers
- `heads/` — Local branches
- `tags/` — Tags
