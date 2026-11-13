# Git Reset

`git reset` is a Git command used to move the current branch reference to another commit and optionally update the staging area and working directory.

It is commonly used to:

* Undo commits.
* Unstage files.
* Remove changes from the staging area.
* Move a branch backward in history.
* Discard local changes.
* Modify the current commit workflow.

Example:

```bash
git reset --soft HEAD~1
```

This moves the current branch back by one commit while keeping the changes staged.

The behavior of `git reset` depends on the reset mode you choose.

# 🗁 Understanding Git Reset

Before using `git reset`, it is important to understand what Git changes during a reset.

A reset can affect three things:

* HEAD and the current branch reference.
* The staging area (index).
* The working directory.

Consider this commit history:

```text
A --- B --- C --- D
                  |
                 HEAD
```

Running:

```bash
git reset --soft HEAD~1
```

Moves the current branch from `D` to `C`.

```text
A --- B --- C --- D
             |
            HEAD
```

The commit `D` is no longer part of the current branch history, but its changes remain staged.

# 🗁 Git's Three Main Areas

Git manages changes across three important areas.

### 🖆 Working Directory

The working directory contains the actual files you edit.

Example:

```text
project/
├── index.html
├── style.css
└── app.js
```

When you modify a file, the changes initially exist in the working directory.

### 🖆 Staging Area

The staging area, also called the index, contains changes prepared for the next commit.

Add a file to the staging area:

```bash
git add index.html
```

### 🖆 Repository

The repository stores committed snapshots and their history.

Create a commit:

```bash
git commit -m "Add homepage"
```

### 🖆 Git's Change Flow

```text
Working Directory
       |
       | git add
       v
  Staging Area
       |
       | git commit
       v
   Repository
```

`git reset` can modify these areas depending on the selected mode.

# 🗁 Understanding HEAD

HEAD is Git's reference to the current checkout.

Normally, HEAD points to the current branch, and that branch points to a commit.

Example:

```text
HEAD
 |
 v
main
 |
 v
Commit D
```

When you run:

```bash
git log --oneline
```

You can see the commit history.

Common HEAD references:

| Reference | Meaning |
| --- | --- |
| `HEAD` | Current commit |
| `HEAD~1` | First parent of HEAD |
| `HEAD~2` | Two first-parent steps behind HEAD |
| `HEAD^` | First parent of HEAD |
| `HEAD^2` | Second parent of a merge commit |
| `HEAD@{1}` | Previous value of HEAD in its reflog |

For example:

```bash
git reset HEAD~1
```

Moves the current branch back one first-parent commit.

# 🗁 Git Reset Modes

Git provides three commonly used reset modes:

* `--soft`
* `--mixed`
* `--hard`

Each mode changes Git's state differently.

# 🗁 Soft Reset

A soft reset moves the current branch reference to another commit but does not modify the staging area or working directory.

Command:

```bash
git reset --soft HEAD~1
```

### 🖆 What Happens?

Suppose your history looks like this:

```text
A --- B --- C
          |
         HEAD
```

After running:

```bash
git reset --soft HEAD~1
```

The branch moves to `B`.

```text
A --- B --- C
     |
    HEAD
```

The changes introduced by commit `C` remain staged.

**Example**:

Create a commit:

```bash
git add .
git commit -m "Add authentication"
```

Undo the commit while keeping its changes staged:

```bash
git reset --soft HEAD~1
```

Check the status:

```bash
git status
```

The changes from the removed commit remain in the staging area.

### 🖆 When to Use Soft Reset

Use `--soft` when:

* You want to undo a commit but keep its changes staged.
* You want to combine multiple commits.
* You want to modify the latest commit.
* You want to create a new commit with a different message.

Example:

```bash
git reset --soft HEAD~1
git commit -m "Improved authentication"
```

# 🗁 Mixed Reset

A mixed reset moves the current branch reference and updates the staging area, but does not modify the working directory.

It is the default reset mode.

Command:

```bash
git reset --mixed HEAD~1
```

Equivalent:

```bash
git reset HEAD~1
```

### 🖆 What Happens?

After a mixed reset:

* The branch moves to the target commit.
* The staging area is updated to match that commit.
* The working directory remains unchanged.

Changes from the removed commit become unstaged modifications.

**Example**

Undo the latest commit:

```bash
git reset --mixed HEAD~1
```

Check the status:

```bash
git status
```

You should see the changes as modified or untracked files, depending on their state.

### 🖆 When to Use Mixed Reset

Use `--mixed` when:

* You want to undo a commit but keep the changes.
* You want to unstage files.
* You want to edit files before committing again.
* You want to reorganize staged changes.

Example:

```bash
git reset HEAD~1
git add index.html
git commit -m "Commit homepage changes"
```

# 🗁 Hard Reset

A hard reset moves the current branch reference, updates the staging area, and updates the working directory to match the target commit.

Command:

```bash
git reset --hard HEAD~1
```

### 🖆 What Happens?

After a hard reset:

* The branch moves to the target commit.
* The staging area matches the target commit.
* Tracked working-tree files are updated to match the target commit.
* Uncommitted changes to tracked files are discarded.

Example:

```text
A --- B --- C
          |
         HEAD
```

Run:

```bash
git reset --hard HEAD~1
```

Result:

```text
A --- B --- C
     |
    HEAD
```

The tracked file changes introduced by `C` are removed from the working directory.

### 🖆 When to Use Hard Reset

Use `--hard` when:

* You intentionally want to discard local tracked-file changes.
* You want your working tree to match a previous commit.
* You want to remove local commits and their tracked-file changes from the current branch state.

Example:

```bash
git reset --hard HEAD~1
```

**Important:** A hard reset can permanently discard uncommitted tracked-file changes. Untracked files are not generally removed by `git reset --hard`.

# 🗁 Comparing Reset Modes

| Behavior | `--soft` | `--mixed` | `--hard` |
| --- | --- | --- | --- |
| Moves branch reference | Yes | Yes | Yes |
| Updates staging area | No | Yes | Yes |
| Updates tracked working files | No | No | Yes |
| Keeps removed commit changes staged | Yes | No | No |
| Keeps removed commit changes unstaged | No | Yes | No |
| Discards uncommitted tracked changes | No | No | Yes |

### 🖆 Visual Comparison

Assume the latest commit contains changes to `app.js`.

```text
                 SOFT
          Commit removed
                 |
                 v
          Changes staged

                MIXED
          Commit removed
                 |
                 v
         Changes unstaged

                HARD
          Commit removed
                 |
                 v
       Tracked changes discarded
```

# 🗁 Reset to a Specific Commit

You can reset your current branch to any commit using its hash.

View commit history:

```bash
git log --oneline
```

Example:

```text
d4e5f6a Add payment feature
c3d4e5f Fix authentication
b2c3d4e Add user registration
a1b2c3d Initial project setup
```

Reset to a specific commit:

```bash
git reset --hard b2c3d4e
```

This moves the current branch to `b2c3d4e` and updates the staging area and tracked working files to match it.

Other modes:

```bash
git reset --soft b2c3d4e
git reset --mixed b2c3d4e
git reset --hard b2c3d4e
```

Choose the mode based on whether you want to preserve staged or unstaged changes.

# 🗁 Reset HEAD

Git provides several ways to reset the latest commit.

### 🖆 Undo the Latest Commit, Keep Changes Staged

```bash
git reset --soft HEAD~1
```

### 🖆 Undo the Latest Commit, Keep Changes Unstaged

```bash
git reset HEAD~1
```

### 🖆 Undo the Latest Commit and Discard Tracked Changes

```bash
git reset --hard HEAD~1
```

### 🖆 Reset Multiple Commits

Move back two commits:

```bash
git reset --soft HEAD~2
```

Move back three commits:

```bash
git reset --mixed HEAD~3
```

The number represents how many first-parent steps to move backward.

# 🗁 Unstage Files

One of the most common uses of `git reset` is removing files from the staging area without deleting their changes.

### 🖆 Unstage a Specific File

```bash
git reset HEAD index.html
```

Modern alternative:

```bash
git restore --staged index.html
```

Both commands remove the file from the staging area while preserving its working-directory changes.

### 🖆 Unstage All Files

```bash
git reset
```

This resets the staging area to match HEAD while leaving working-directory changes untouched.

Modern alternative:

```bash
git restore --staged .
```

**Example**

Stage files:

```bash
git add .
```

Unstage one file:

```bash
git reset HEAD app.js
```

Check the result:

```bash
git status
```

`app.js` is now unstaged, but its modifications remain in the working directory.

# 🗁 Reset a Specific File

You can reset a specific file without moving the current branch reference.

### 🖆 Reset a File in the Staging Area

```bash
git reset HEAD -- app.js
```

This makes the staging-area version of `app.js` match HEAD.

The working-directory version remains unchanged.

### 🖆 Reset a File to a Specific Commit

```bash
git reset <commit-hash> -- app.js
```

This updates the staging-area version of `app.js` to match the specified commit.

It does not move the current branch reference.

### 🖆 Restore a File in the Working Directory

To discard unstaged changes to a tracked file:

```bash
git restore app.js
```

To restore both the staging area and working directory from HEAD:

```bash
git restore --source=HEAD --staged --worktree app.js
```

Be careful: restoring the working-tree version discards its current uncommitted modifications.

# 🗁 Reset to Remote Branch

Sometimes your local branch needs to match the latest remote branch exactly.

First, fetch the latest remote information:

```bash
git fetch origin
```

Then reset the current branch:

```bash
git reset --hard origin/main
```

This makes the current branch and tracked working files match `origin/main`.

### 🖆 What Happens?

* Local commits not present in `origin/main` are removed from the current branch's history.
* Staged changes are discarded.
* Uncommitted changes to tracked files are discarded.
* Untracked files are generally left untouched.

Check the result:

```bash
git status
git log --oneline -5
```

Use this workflow only when you intentionally want to discard local divergence and changes.

# 🗁 Reset vs Revert vs Checkout

These commands have different purposes.

| Command | Purpose |
| --- | --- |
| `git reset` | Moves the current branch reference and can update the index and working tree |
| `git revert` | Creates a new commit that reverses an earlier commit |
| `git checkout` | Switches branches or restores files; legacy command with multiple responsibilities |
| `git restore` | Restores files in the working tree or staging area |
| `git switch` | Switches branches or creates a branch |

**Reset Example**

```bash
git reset --soft HEAD~1
```

Moves the current branch back one commit.

**Revert Example**

```bash
git revert HEAD
```

Creates a new commit that reverses the latest commit.

**Restore Example**

```bash
git restore app.js
```

Discards unstaged changes in `app.js`.

**Switch Example**

```bash
git switch develop
```

Switches to the `develop` branch.

Use `git revert` when you need to undo a commit in shared history without rewriting that history.

# 🗁 Recovering From Git Reset

Git maintains a reflog that records updates to references such as HEAD and branch pointers.

Even after a reset, a previous commit may still be recoverable through the reflog.

### 🖆 View Reflog

```bash
git reflog
```

Example:

```text
a1b2c3d HEAD@{0}: reset: moving to HEAD~1
d4e5f6a HEAD@{1}: commit: Add payment feature
c3d4e5f HEAD@{2}: commit: Fix authentication
```

Here, `d4e5f6a` was the commit before the reset.

### 🖆 Recover a Previous Commit

Create a branch pointing to the previous commit:

```bash
git branch recovery-branch d4e5f6a
```

Or reset the current branch back to it:

```bash
git reset --hard d4e5f6a
```

The branch approach is useful because it preserves a recovery reference without immediately changing your current branch.

### 🖆 Recover Using HEAD Reflog

You can also use:

```bash
git reset --hard HEAD@{1}
```

This moves the current branch back to the commit recorded at that reflog position.

Reflog entries are local and expire according to Git's reflog expiration settings. Uncommitted changes discarded by a hard reset may not be recoverable through reflog.

# 🗁 Git Reset Internals

Understanding the internal behavior of `git reset` helps explain why its modes behave differently.

A reset primarily involves three operations:

* Move the current branch reference.
* Update the index.
* Update the working directory.

### 🖆 Soft Reset Internals

```bash
git reset --soft HEAD~1
```

Only the current branch reference moves.

```text
HEAD / Branch     → Target Commit
Index             → Unchanged
Working Directory → Unchanged
```

### 🖆 Mixed Reset Internals

```bash
git reset --mixed HEAD~1
```

The branch reference moves and the index is updated.

```text
HEAD / Branch     → Target Commit
Index             → Target Commit
Working Directory → Unchanged
```

### 🖆 Hard Reset Internals

```bash
git reset --hard HEAD~1
```

All three are updated.

```text
HEAD / Branch     → Target Commit
Index             → Target Commit
Working Directory → Target Commit
```

For a path-specific reset:

```bash
git reset HEAD -- app.js
```

Git updates the index entry for `app.js` without moving the branch reference.

# 🖆 Common Problems and Solutions

### 🖆 Accidentally Used `git reset --hard`

Check the reflog:

```bash
git reflog
```

If the lost commit is available, recover it:

```bash
git branch recovery-branch <commit-hash>
```

### 🖆 Accidentally Unstaged Files

If you only used:

```bash
git reset
```

Your working-directory changes are still available.

Stage them again:

```bash
git add .
```

### 🖆 Local Branch Is Behind or Different From Remote

Fetch remote updates:

```bash
git fetch origin
```

Inspect the differences:

```bash
git log --oneline HEAD..origin/main
git log --oneline origin/main..HEAD
```

If you intentionally want the local branch to match the remote:

```bash
git reset --hard origin/main
```

### 🖆 Accidentally Removed a Commit

Inspect the reflog:

```bash
git reflog
```

Find the previous commit hash and recover it:

```bash
git branch recovery-branch <commit-hash>
```

### 🖆 Untracked Files Remain After Hard Reset

`git reset --hard` does not generally remove untracked files.

Inspect them:

```bash
git status --short
```

If you intentionally want to remove untracked files, preview first:

```bash
git clean -n
```

Then remove them:

```bash
git clean -f
```

To remove untracked directories too:

```bash
git clean -fd
```

# 🖆 Best Practices

* Understand the difference between `--soft`, `--mixed`, and `--hard`.
* Prefer `--soft` when you want to redo a commit without losing staged changes.
* Use `--mixed` when you want to unstage changes while preserving working files.
* Use `--hard` only when you intentionally want to discard tracked-file changes.
* Inspect `git status` before resetting.
* Use `git log --oneline` to identify the correct target commit.
* Use `git reflog` when you need to recover a previous branch position.
* Avoid rewriting published shared history unless collaborators agree.
* Prefer `git revert` for undoing commits that have already been shared.
* Consider creating a backup branch before a complicated reset.

# 🖆 Practical Exercises

* Create a new repository and make three commits.
* Perform a soft reset to undo the latest commit.
* Check which changes remain staged.
* Perform a mixed reset and inspect the working directory.
* Perform a hard reset in a disposable test repository.
* Unstage a single file using `git reset`.
* Unstage all files using `git reset`.
* Reset a specific file to a previous commit.
* Reset your local branch to `origin/main` in a test repository.
* Use `git reflog` to locate a previous commit.
* Create a recovery branch from a reflog entry.
* Compare `git reset` and `git revert` by undoing commits in separate test branches.

# 🖆 Interview Questions

**What is `git reset`?**

A Git command used to move the current branch reference to another commit and optionally update the staging area and working directory.

**What are the three main reset modes?**

* `--soft`
* `--mixed`
* `--hard`

**What does `git reset --soft HEAD~1` do?**

Moves the current branch back one commit while keeping the removed commit's changes staged.

**What does `git reset --mixed HEAD~1` do?**

Moves the current branch back one commit and unstages the changes while keeping them in the working directory.

**What does `git reset --hard HEAD~1` do?**

Moves the current branch back one commit and updates the staging area and tracked working files to match the target commit.

**What is the default reset mode?**

`--mixed`.

**How do you unstage a file?**

```bash
git reset HEAD filename
```

Alternatively:

```bash
git restore --staged filename
```

**How do you undo the last commit but keep its changes?**

```bash
git reset --soft HEAD~1
```

**How do you recover a commit after resetting?**

Inspect the reflog:

```bash
git reflog
```

Then create a recovery branch or reset to the previous commit.

**What is the difference between reset and revert?**

Reset moves the current branch reference and can rewrite local history. Revert creates a new commit that reverses an earlier commit.

**Does `git reset --hard` remove untracked files?**

No. It updates tracked files and the index, but does not generally remove untracked files.

**What is `HEAD~1`?**

The first-parent commit before the current commit.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [Git Documentation — git-reset](https://git-scm.com/docs/git-reset)
* [Git Documentation — git-reflog](https://git-scm.com/docs/git-reflog)
* [Git Documentation — git-restore](https://git-scm.com/docs/git-restore)
* [Git Documentation — git-revert](https://git-scm.com/docs/git-revert)
* [Git Documentation — git-clean](https://git-scm.com/docs/git-clean)
* [Git Book — Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)
