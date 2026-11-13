# Git Revert

`git revert` is a Git command used to undo the changes introduced by an existing commit.

Instead of removing a commit from history, Git creates a new commit that reverses the changes made by the selected commit.

Example:

```bash
git revert HEAD
```

This creates a new commit that reverses the changes introduced by the latest commit.

Git revert is particularly useful when working with shared repositories because it preserves the existing commit history.

# 🗁 Understanding Git Revert

Every Git commit represents a snapshot of the repository.

Suppose your history looks like this:

```text
A --- B --- C --- D
                  |
                 HEAD
```

Assume commit `D` introduced a bug.

You can revert it:

```bash
git revert HEAD
```

Git creates a new commit `E` that reverses the changes introduced by `D`.

```text
A --- B --- C --- D --- E
                  |     |
              Original Revert
               Commit  Commit
```

Important points:

* Commit `D` remains in history.
* Git creates a new commit `E`.
* Commit `E` applies the inverse of the changes introduced by `D`.
* The repository history remains intact.

A revert does not guarantee that the entire repository returns to the exact state before the original commit if subsequent commits depend on or modify the same files.

# 🗁 How Git Revert Works

Git compares the selected commit with its parent and calculates the changes introduced by that commit.

It then attempts to apply the inverse of those changes to the current working tree and staging area.

For example:

Original commit:

```diff
- const timeout = 3000;
+ const timeout = 5000;
```

Reverting that commit applies the inverse change:

```diff
- const timeout = 5000;
+ const timeout = 3000;
```

Git then creates a new commit containing the reversal.

The original commit is not deleted or modified.

# 🗁 Git Revert vs Git Reset

Both commands can undo changes, but they work differently.

| Feature | Git Revert | Git Reset |
| --- | --- | --- |
| Creates a new commit | Yes, by default | No |
| Removes existing commits from current branch history | No | Can move branch backward |
| Preserves existing commit history | Yes | Can rewrite local history |
| Suitable for shared branches | Generally yes | Usually avoid rewriting shared history |
| Can undo a specific commit | Yes | Primarily moves branch to a target state |
| Common use | Safely undo published changes | Manage local commits and staging |

### 🖆 Revert Example

```bash
git revert HEAD
```

Creates a new commit that reverses the latest commit.

### 🖆 Reset Example

```bash
git reset --soft HEAD~1
```

Moves the current branch back one commit while keeping the changes staged.

### 🖆 When to Use Revert

Use `git revert` when:

* The commit has already been pushed.
* Other developers may have based work on the commit.
* You need to undo a production change.
* You want to preserve repository history.
* You need an auditable record of the correction.

### 🖆 When to Use Reset

Use `git reset` when:

* You are correcting local commits.
* You want to unstage changes.
* You want to reorganize local history.
* You intentionally want to move a branch reference.

# 🗁 Reverting the Latest Commit

To revert the latest commit:

```bash
git revert HEAD
```

Git opens the configured editor with a default revert commit message.

Save and close the editor to create the revert commit.

### 🖆 Use the Default Revert Message Without Opening an Editor

```bash
git revert --no-edit HEAD
```

**Example:**

Original history:

```text
A --- B --- C
          |
         HEAD
```

Run:

```bash
git revert HEAD
```

Result:

```text
A --- B --- C --- D
```

Commit `D` reverses the changes introduced by `C`.

Check the history:

```bash
git log --oneline
```

# 🗁 Reverting a Specific Commit

You can revert any existing commit by providing its commit hash.

Find the commit:

```bash
git log --oneline
```

Example:

```text
d4e5f6a Add payment integration
c3d4e5f Fix authentication
b2c3d4e Add user registration
a1b2c3d Initial project setup
```

Revert the payment integration commit:

```bash
git revert d4e5f6a
```

Git creates a new commit that reverses the changes introduced by `d4e5f6a`.

The selected commit does not need to be the latest commit.

However, if later commits modify the same code, Git may encounter conflicts or the reversal may not produce the exact earlier file state.

# 🗁 Reverting Multiple Commits

Git allows you to revert multiple commits.

### 🖆 Revert Several Specific Commits

```bash
git revert <commit-1> <commit-2>
```

Git processes the commits in the specified order.

For dependent commits, consider the order carefully because reverting an earlier change before a later dependent change can produce conflicts.

### 🖆 Revert a Commit Range

Example:

```bash
git revert HEAD~3..HEAD
```

This selects the commits reachable from HEAD but not reachable from `HEAD~3`.

For a linear history, this selects the latest three commits.

The range is processed in reverse chronological order by Git's revision walk, which normally means the newest selected commit is reverted first.

### 🖆 Revert Multiple Commits as One Revert Commit

Use `--no-commit`:

```bash
git revert --no-commit HEAD~3..HEAD
```

After Git applies the inverse changes, create one commit:

```bash
git commit -m "Revert recent changes"
```

This is useful when you want a single commit representing the combined reversal.

# 🗁 Reverting Without Committing

By default, `git revert` creates a new commit.

You can prevent automatic commit creation using `--no-commit`.

Command:

```bash
git revert --no-commit HEAD
```

The inverse changes are applied to the working tree and staging area, but Git does not create a commit.

Inspect the changes:

```bash
git status
git diff --cached
```

You can then modify the result if needed.

Create the commit manually:

```bash
git commit -m "Revert latest changes"
```

### 🖆 When to Use `--no-commit`

* When reverting multiple commits together.
* When you want to inspect the inverse changes before committing.
* When you want to combine the reversal with other changes.
* When you need a custom commit message.

# 🗁 Reverting Merge Commits

A merge commit has more than one parent.

Example:

```text
          C --- D
         /       \
A --- B ---------- M
```

Here, `M` is a merge commit with two parents.

To revert a merge commit, Git needs to know which parent should be treated as the mainline.

Use:

```bash
git revert -m 1 <merge-commit-hash>
```

The `-m` option specifies the mainline parent number.

### 🖆 Understanding Mainline Parents

For a typical merge:

```text
          feature commits
               |
               v
A --- B ------- M
      \        /
       \------/
```

If the first parent of `M` is the branch that was checked out when the merge happened, then:

```bash
git revert -m 1 M
```

Reverses the changes introduced by merging the other parent into the first parent.

Inspect the parents:

```bash
git show --no-patch --pretty=raw <merge-commit-hash>
```

### 🖆 💡IMPORTANT CONSIDERATION

Reverting a merge commit does not simply erase the merge from history.

The merge commit remains in history, and Git records a new commit that reverses its changes relative to the selected mainline parent.

This also affects future merges: Git still considers the original merged commits part of the branch history.

If you later want to bring those changes back, you may need to revert the revert commit or create additional commits containing the desired changes.

# 🗁 Reverting a Revert

Sometimes you may need to restore changes that were previously reverted.

You can do this by reverting the revert commit.

Example history:

```text
A --- B --- C --- D
          |     |
       Feature  Revert
```

Suppose:

* `C` introduced a feature.
* `D` reverted that feature.

To restore the feature:

```bash
git revert D
```

Git creates another commit that reverses the previous reversal.

Result:

```text
A --- B --- C --- D --- E
          |     |     |
       Feature Revert Restore
```

The original feature changes are effectively reintroduced.

This is useful when a bug fix or deployment issue has been resolved and the previously reverted feature is ready to return.

# 🗁 Handling Revert Conflicts

A revert can produce conflicts when the inverse changes cannot be applied cleanly.

This commonly happens when:

* Later commits modified the same lines.
* The selected commit changed files that have since changed again.
* Multiple dependent commits are being reverted.
* The current branch has diverged from the original commit's context.

**Example:**

Run:

```bash
git revert <commit-hash>
```

If a conflict occurs, Git reports the affected files.

Check the status:

```bash
git status
```

Open the conflicting files and resolve the conflict markers.

Example:

```text
<<<<<<< HEAD
Current version
=======
Reverted version
>>>>>>> parent of <commit-hash>
```

Edit the file to produce the desired final content.

Stage the resolved files:

```bash
git add .
```

Continue the revert:

```bash
git revert --continue
```

### 🖆 Abort a Revert

If you want to cancel the current revert operation:

```bash
git revert --abort
```

Git attempts to restore the state from before the revert operation began.

### 🖆 Skip a Revert

If a multi-commit revert is in progress and you want to skip the current commit:

```bash
git revert --skip
```

Use this only when skipping that reversal is intentional.

# 🗁 Git Revert Options

| Option               | Purpose                                                                               |
| -------------------- | ------------------------------------------------------------------------------------- |
| `--no-edit`          | Use the default revert commit message without opening an editor                       |
| `--edit`             | Open the editor to modify the commit message                                          |
| `--no-commit`        | Apply inverse changes without creating a commit                                       |
| `-m <parent-number>` | Select the mainline parent when reverting a merge commit                              |
| `--continue`         | Continue after resolving conflicts                                                    |
| `--abort`            | Abort the current revert operation                                                    |
| `--skip`             | Skip the current commit during a multi-commit revert                                  |
| `--quit`             | Forget the current revert operation state without resetting the index or working tree |

Check the full command documentation:

```bash
git revert --help
```

# 🗁 Understanding the Revert Workflow

A typical revert workflow looks like this:

```text
       Identify Commit
              |
              v
       git log --oneline
              |
              v
        Select Commit
              |
              v
       git revert <hash>
              |
              v
       Conflict Occurred?
          /       \
        Yes        No
         |          |
         v          v
   Resolve Files   Revert Commit
         |
         v
      git add
         |
         v
  git revert --continue
         |
         v
      Verify Changes
         |
         v
      Push Changes
```

After a successful revert:

```bash
git log --oneline
git status
git push origin main
```

If you are working on a feature branch, push that branch instead.

# 🖆 Common Use Cases

### 🖆 Undo a Buggy Production Commit

Suppose a commit introduced a production bug.

Find the commit:

```bash
git log --oneline
```

Revert it:

```bash
git revert <commit-hash>
```

Push the new revert commit:

```bash
git push origin main
```

The original commit remains available in history.

### 🖆 Undo a Feature That Was Already Merged

Find the merge commit:

```bash
git log --oneline --graph
```

Revert the merge:

```bash
git revert -m 1 <merge-commit-hash>
```

The selected mainline parent determines which side of the merge is retained.

### 🖆 Undo Several Recent Commits

For a linear history, inspect the range:

```bash
git log --oneline HEAD~3..HEAD
```

Then revert the range:

```bash
git revert HEAD~3..HEAD
```

Review the results and resolve any conflicts.

### 🖆 Restore a Previously Reverted Feature

Find the commit that reverted the feature:

```bash
git log --oneline
```

Revert that revert commit:

```bash
git revert <revert-commit-hash>
```

# 🖆 Common Problems and Solutions

### 🖆 Revert Conflicts Occur

Check conflicting files:

```bash
git status
```

Resolve the conflicts, stage the files, and continue:

```bash
git add .
git revert --continue
```

### 🖆 Wrong Commit Was Reverted

If the revert commit has not been published and you want to undo the operation, inspect the current state and use the appropriate recovery approach.

If the revert commit has already been shared, you can usually reverse it with another revert:

```bash
git revert <revert-commit-hash>
```

This preserves the shared history.

### 🖆 Cannot Revert a Merge Commit

Git requires a mainline parent.

Use:

```bash
git revert -m 1 <merge-commit-hash>
```

Confirm that parent `1` is the intended mainline before proceeding.

### 🖆 Revert Operation Is Still in Progress

Check the repository status:

```bash
git status
```

Continue after resolving conflicts:

```bash
git revert --continue
```

Or cancel the operation:

```bash
git revert --abort
```

### 🖆 Changes Are Not Exactly the Same as Before

A revert applies inverse changes to the current state. It does not necessarily restore the entire repository to an earlier snapshot.

Review the resulting changes:

```bash
git diff
git diff --cached
git show --stat HEAD
```

# 🖆 Best Practices

* Prefer `git revert` for undoing commits already pushed to shared branches.
* Inspect commit history before selecting a commit to revert.
* Review changes before pushing a revert commit.
* Use `--no-commit` when you need to combine multiple reversals.
* Understand merge parents before reverting merge commits.
* Resolve conflicts carefully instead of blindly accepting one side.
* Use meaningful revert commit messages.
* Avoid rewriting shared history to undo published changes.
* Test the application after reverting production changes.
* Remember that reverting a merge does not erase the merged commits from history.

# 🖆 Practical Exercises

* Create a repository and make several commits.
* Revert the latest commit.
* Revert an older commit using its hash.
* Compare the repository history before and after reverting.
* Revert multiple commits using a revision range.
* Revert multiple commits with `--no-commit`.
* Create a merge commit and revert it using `-m 1`.
* Revert a revert commit to restore previously removed changes.
* Create a conflict scenario and resolve it during a revert.
* Abort a revert operation.
* Push a revert commit to a remote repository.
* Compare `git reset` and `git revert` using separate test branches.

# 🖆 Interview Questions

**What is `git revert`?**

A Git command that creates a new commit to reverse the changes introduced by an existing commit.

**Does Git revert delete the original commit?**

No. The original commit remains in repository history.

**What is the difference between reset and revert?**

Reset moves the current branch reference and can rewrite local history. Revert creates a new commit that reverses previous changes.

**Why is revert useful in shared repositories?**

It preserves existing history and allows developers to undo published changes without removing commits that others may already depend on.

**How do you revert the latest commit?**

```bash
git revert HEAD
```

**How do you revert a specific commit?**

```bash
git revert <commit-hash>
```

**What does `--no-commit` do?**

It applies the inverse changes without automatically creating a revert commit.

**How do you revert a merge commit?**

```bash
git revert -m 1 <merge-commit-hash>
```

The `-m` option specifies the mainline parent.

**How do you continue after resolving revert conflicts?**

```bash
git revert --continue
```

**How do you cancel an ongoing revert?**

```bash
git revert --abort
```

**How do you restore changes that were previously reverted?**

Revert the commit that performed the original revert:

```bash
git revert <revert-commit-hash>
```

**Does reverting a commit restore the entire repository to its previous state?**

Not necessarily. It applies inverse changes to the current state, which may include later modifications.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

Official References

* [Git Documentation — git-revert](https://git-scm.com/docs/git-revert)
* [Git Documentation — git-reset](https://git-scm.com/docs/git-reset)
* [Git Documentation — git-log](https://git-scm.com/docs/git-log)
* [Git Documentation — git-merge](https://git-scm.com/docs/git-merge)
* [Pro Git Book — Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)
