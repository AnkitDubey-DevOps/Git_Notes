# Explore Git Internals Live

Try these commands during a live session:

    # Create repo and file
    git init
    echo "Hello Git" > hello.txt
    git add hello.txt
    git commit -m "Initial commit"

    # Inspect Git internals
    ls .git/objects
    git cat-file -p HEAD
    git cat-file -p <commit hash>
    git cat-file -p <tree hash>

# Git = Snapshots + DAG + Content Hashing

| Core Concept | Benefit |
|---|---|
| Snapshot-based | Ensures complete project state at every commit |
| DAG structure | Enables powerful merge/rebase/branch operations |
| SHA-1 hashing | Ensures data integrity and deduplication |
| Local repository | Fast operations, offline support |
| Immutable commits | Clean audit trail and history |

# Summary (Key Takeaways)

- Git is not just a VCS — it's a snapshot engine with a content-addressed filesystem
- Every commit = complete tree state + metadata
- Blobs store content, trees organize it, commits record it, refs track it
- Git’s internals (SHA-1 + DAG) make it robust, fast, and scalable
