# Phase 5: Analysis
Q5.1: Implementing pes checkout <branch>
To implement branch switching, the following logic must be executed:

Reference Update: The .pes/HEAD file must be updated to point to the new branch reference (e.g., changing from ref: refs/heads/main to ref: refs/heads/feature).

Working Directory Sync: The current files in the working directory must be removed and replaced with the specific versions of files tracked in the tree object of the target branch's latest commit.

Complexity: The operation is complex because it must handle "dirty" files—uncommitted changes that only exist in the working directory. Overwriting these without a warning would lead to permanent data loss.

Q5.2: Detecting "Dirty Working Directory" Conflicts
A "dirty" state can be detected using metadata comparisons without re-hashing all files:

Working Dir vs. Index: Compare the current metadata (mtime and size) of files in the working directory against the values stored in the Index. Differences indicate unstaged changes.

Index vs. HEAD: Compare the hash stored in the Index for a file against the hash in the Object Store associated with the current HEAD commit. Differences indicate staged but uncommitted changes.

Conflict Detection: If either of these stages shows a difference, the directory is "dirty". If these dirty files also differ in the target branch, a conflict is flagged.

Q5.3: Detached HEAD States
Definition: A "Detached HEAD" occurs when .pes/HEAD contains a raw commit hash directly instead of a branch reference like refs/heads/main.

Committing: Commits made in this state work normally but no branch pointer moves forward to track them. The new commits exist in the object store but are not "anchored" to a branch name.

Recovery: If a user switches away, these commits become "orphaned". To recover them, the user must find the specific commit hash (from terminal history or a reflog) and manually create a new branch pointing to that hash.



