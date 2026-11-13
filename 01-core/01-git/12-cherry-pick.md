# Git Cherry-Pick

`git cherry-pick` is a Git command used to apply the changes introduced by an existing commit to the current branch.

It allows you to take a specific commit from one branch and apply its changes to another branch without merging the entire source branch.

Example:

```bash
git cherry-pick <commit-hash>
```

Cherry-pick is particularly useful when:

* You need a specific bug fix from another branch.
* You want to transfer a small feature between branches.
* You need to apply a hotfix to multiple release branches.
* You want selected commits without merging unrelated changes.

# 🗁 Understanding Git Cherry-Pick

Consider two branches:

```text
main:       A --- B --- C
                  \
feature:           D --- E
```

Suppose commit `D` contains an important bug fix, but you do not want to merge the entire feature branch.

You can apply commit `D` to `main`:

```bash
git switch main
git cherry-pick <commit-D>
```

Result:

```text
main:       A --- B --- C --- D'
                  \
feature:           D --- E
```

`D'` contains the changes introduced by `D`, but it is a new commit with a different commit hash.

The original commit `D` remains on the feature branch.

# 🗁 How Cherry-Pick Works

Cherry-pick identifies the changes introduced by a selected commit relative to its parent.

Git then attempts to apply those changes to the current branch.

If the application succeeds, Git creates a new commit.

For example, the original commit introduces:

```diff
- const timeout = 3000;
+ const timeout = 5000;
```

Cherry-picking that commit onto another branch attempts to apply the same change there.

The resulting commit has:

* The same patch content, if applied without modification.
* A different commit hash.
* The current branch's history as its parent.
* The original commit's author information, normally preserved.
* A new committer identity and timestamp.

Cherry-pick copies changes, not the original commit object or its ancestry.

# 🗁 Cherry-Pick vs Merge vs Rebase

These commands are used to integrate changes, but their behavior differs.

| Feature | Cherry-Pick | Merge | Rebase |
| --- | --- | --- | --- |
| Purpose | Apply selected commits | Combine branch histories | Replay commits onto another base |
| Selective commits | Yes | Normally merges branch changes | Replays selected branch commits |
| Creates new commits | Yes | Usually creates a merge commit, unless fast-forwarded | Recreates commits |
| Preserves original commit hashes | No for copied commits | Yes for existing commits | No for replayed commits |
| Common use | Individual fixes | Integrating branches | Cleaning up branch history |

### 🖆 Cherry-Pick

```bash
git cherry-pick <commit-hash>
```

Applies a selected commit to the current branch.

### 🖆 Merge

```bash
git merge feature
```

Integrates the feature branch into the current branch.

### 🖆 Rebase

```bash
git rebase main
```

Replays the current branch's commits on top of `main`.

Use cherry-pick when you need specific changes rather than an entire branch.

# 🗁 Basic Cherry-Pick

The basic workflow involves identifying a commit, switching to the destination branch, and applying the commit.

### 🖆 View Commit History

```bash
git log --oneline --all --graph
```

Example:

```text
a8f34d2 Add payment validation
b7e21c9 Fix login bug
c5d12a8 Add user registration
```

### 🖆 Switch to the Destination Branch

```bash
git switch main
```

### 🖆 Cherry-Pick the Commit

```bash
git cherry-pick b7e21c9
```

### 🖆 Verify the Result

```bash
git log --oneline
```

The selected changes are now included in the current branch through a new commit.

# 🗁 Cherry-Pick a Specific Commit

You can cherry-pick any reachable commit by providing its hash.

Example:

```bash
git cherry-pick a8f34d2
```

Git applies the changes introduced by that commit to the current branch.

You do not need to check out the source branch first, provided the commit is available in your repository.

**Example:** Transfer a Bug Fix

Suppose your repository contains:

```text
main
feature/authentication
```

A developer fixes a login issue on `feature/authentication`.

Find the commit:

```bash
git log feature/authentication --oneline
```

Switch to `main`:

```bash
git switch main
```

Apply the fix:

```bash
git cherry-pick <bug-fix-commit>
```

The bug fix is now included in `main` without merging the entire authentication branch.

# 🗁 Cherry-Pick Multiple Commits

You can apply multiple commits in a single command.

Example:

```bash
git cherry-pick <commit-1> <commit-2> <commit-3>
```

Git processes the commits in the order specified.

Each successful cherry-pick normally creates a separate commit.

Example:

```bash
git switch main

git cherry-pick a1b2c3d d4e5f6a g7h8i9j
```

This applies the selected commits sequentially.

If a conflict occurs, Git stops and requires you to resolve or abort the operation before proceeding.

# 🗁 Cherry-Pick a Commit Range

You can cherry-pick a range of commits using Git revision syntax.

Example:

```bash
git cherry-pick A..B
```

This selects commits reachable from `B` but not reachable from `A`.

For a simple linear history, it selects commits after `A` through `B`, excluding `A`.

Example:

```text
A --- B --- C --- D --- E
```

Command:

```bash
git cherry-pick B..E
```

Selected commits:

```text
C, D, E
```

Git processes the selected commits in its revision-walk order, normally oldest first for this operation.

### 🖆 Include the Starting Commit

To include `A` itself:

```bash
git cherry-pick A^..B
```

For a linear history, this selects `A` and all commits through `B`.

### 🖆 Important Consideration

Commit ranges can be more complex in branched histories because Git revision ranges describe reachability, not simply visual positions on a graph.

Inspect the selected commits before applying them:

```bash
git log --oneline A..B
```

# 🗁 Cherry-Pick Without Committing

By default, cherry-pick creates a new commit after successfully applying the changes.

You can apply changes without automatically creating a commit using `--no-commit`.

Command:

```bash
git cherry-pick --no-commit <commit-hash>
```

Short form:

```bash
git cherry-pick -n <commit-hash>
```

The changes are applied to the index and working tree, but no commit is created.

Check the result:

```bash
git status
git diff --cached
```

You can then modify the changes and create your own commit:

```bash
git commit -m "Apply selected bug fix"
```

### 🖆 Cherry-Pick Multiple Commits Without Committing

```bash
git cherry-pick --no-commit <commit-1> <commit-2>
```

This is useful when you want to combine selected changes into one commit.

# 🗁 Cherry-Pick and Edit Commit Messages

### 🖆 Use the Original Commit Message

By default, Git uses the original commit message when creating the new commit.

```bash
git cherry-pick <commit-hash>
```

### 🖆 Edit the Commit Message

Use `--edit`:

```bash
git cherry-pick --edit <commit-hash>
```

Git opens the configured editor so you can modify the commit message.

### 🖆 Keep the Original Message Without Editing

```bash
git cherry-pick --no-edit <commit-hash>
```

# 🗁 Cherry-Pick Merge Commits

A merge commit has multiple parents.

Example:

```text
          C --- D
         /       \
A --- B ---------- M
```

Here, `M` is a merge commit.

Cherry-picking a merge commit requires specifying which parent should be treated as the mainline.

Command:

```bash
git cherry-pick -m 1 <merge-commit-hash>
```

The `-m` option specifies the mainline parent number.

### 🖆 Understanding Mainline Parents

For a typical merge:

* Parent `1` is the branch that was checked out when the merge happened.
* Parent `2` is commonly the branch that was merged in.

Inspect the merge commit:

```bash
git show --no-patch --pretty=raw <merge-commit-hash>
```

### 🖆 What Does `-m 1` Mean?

```bash
git cherry-pick -m 1 <merge-commit-hash>
```

Git calculates the changes introduced by the merge relative to its first parent and applies those changes to the current branch.

This can bring the merged branch's combined changes into another branch.

Choose the mainline parent carefully because choosing a different parent produces a different patch.

# 🗁 Handling Cherry-Pick Conflicts

Cherry-pick conflicts occur when Git cannot apply the selected commit cleanly.

Common reasons:

* The destination branch modified the same lines.
* The source commit depends on earlier changes that are missing.
* The destination branch has different file structures.
* Multiple selected commits modify overlapping code.

**Example:**

Run:

```bash
git cherry-pick <commit-hash>
```

If a conflict occurs, Git reports the affected files.

Check the status:

```bash
git status
```

Open the conflicting files.

Example conflict:

```text
<<<<<<< HEAD
const timeout = 3000;
=======
const timeout = 5000;
>>>>>>> <commit-hash>
```

Resolve the conflict by editing the file into the desired final state.

Stage the resolved files:

```bash
git add .
```

Continue cherry-picking:

```bash
git cherry-pick --continue
```

### 🖆 Abort Cherry-Pick

If you want to cancel the current cherry-pick operation:

```bash
git cherry-pick --abort
```

Git attempts to restore the state from before the operation began.

### 🖆 Skip a Commit

When cherry-picking multiple commits, you can skip the current commit:

```bash
git cherry-pick --skip
```

Use this when you intentionally do not want to apply the current commit.

# 🗁 Cherry-Pick Options

| Option | Purpose |
| --- | --- |
| `-e`, `--edit` | Edit the commit message |
| `-n`, `--no-commit` | Apply changes without creating a commit |
| `-x` | Append the original commit hash to the new commit message |
| `-m`, `--mainline` | Select the mainline parent for a merge commit |
| `--continue` | Continue after resolving conflicts |
| `--abort` | Abort the current cherry-pick operation |
| `--skip` | Skip the current commit |
| `--quit` | Forget the current operation state without resetting the index or working tree |
| `--no-edit` | Use the existing commit message without opening an editor |

### 🖆 Using `-x`

The `-x` option adds a reference to the original commit hash in the new commit message.

Example:

```bash
git cherry-pick -x <commit-hash>
```

The resulting message may look like:

```text
Fix login validation

(cherry picked from commit abc1234...)
```

This is useful when tracking changes transferred between branches.

# 🗁 Understanding Commit References

Cherry-pick accepts several forms of commit references.

### 🖆 Commit Hash

```bash
git cherry-pick a1b2c3d
```

### 🖆 Branch Reference

```bash
git cherry-pick feature/authentication
```

This applies the commit currently referenced by that branch. It does not merge the entire branch.

### 🖆 Tag Reference

```bash
git cherry-pick v1.0.0
```

This applies the commit referenced by the tag.

### 🖆 Relative Commit Reference

```bash
git cherry-pick HEAD~2
```

This applies the changes introduced by the commit two first-parent steps behind HEAD.

# 🗁 Common Use Cases

### 🖆 Apply a Bug Fix to Another Branch

Suppose a bug fix exists on `develop`, but you also need it on `main`.

Find the commit:

```bash
git log develop --oneline
```

Switch branches:

```bash
git switch main
```

Apply the fix:

```bash
git cherry-pick <bug-fix-commit>
```

### 🖆 Apply a Hotfix to Multiple Release Branches

Suppose you maintain:

```text
main
release/1.x
release/2.x
```

A bug fix is committed on `main`.

Apply it to another release branch:

```bash
git switch release/1.x
git cherry-pick <hotfix-commit>
```

Then apply it to the second release branch:

```bash
git switch release/2.x
git cherry-pick <hotfix-commit>
```

Each branch receives a new commit containing the selected changes.

The changes may need adjustment if the branches have diverged significantly.

### 🖆 Transfer a Commit From a Feature Branch

```bash
git switch main
git cherry-pick <feature-commit>
```

This transfers one selected commit without integrating the entire feature branch.

### 🖆 Combine Multiple Changes Into One Commit

```bash
git cherry-pick --no-commit <commit-1> <commit-2>
git commit -m "Apply selected improvements"
```

This applies multiple commits' changes and records them together in a new commit.

# 🗁 Common Problems and Solutions

### 🖆 Commit Is Already Applied

Sometimes the selected changes already exist on the destination branch.

Git may report that the cherry-pick is empty.

Check the current status:

```bash
git status
```

If the changes are already present and you do not need a new commit:

```bash
git cherry-pick --skip
```

If you intentionally need an empty commit to record the operation, Git supports:

```bash
git commit --allow-empty
```

### 🖆 Cherry-Pick Conflicts

Check conflicting files:

```bash
git status
```

Resolve the conflicts and stage the files:

```bash
git add .
git cherry-pick --continue
```

### 🖆 Cherry-Pick Was Applied to the Wrong Branch

If the new commit has not been pushed and is the latest commit, you can undo it locally:

```bash
git reset --hard HEAD~1
```

Only use this if you are certain the commit is the one you want to remove and you have no local changes to preserve.

If the commit has already been pushed or shared, prefer reverting it:

```bash
git revert <cherry-picked-commit>
```

### 🖆 Cherry-Pick Produces Unexpected Changes

The selected commit may depend on earlier commits that are not present on the destination branch.

Inspect the original commit:

```bash
git show <commit-hash>
```

Inspect its parent history:

```bash
git log --oneline <commit-hash>^..<commit-hash>
```

Identify missing dependencies before applying the changes.

### 🖆 Cherry-Pick Operation Is Still in Progress

Check the status:

```bash
git status
```

Continue:

```bash
git cherry-pick --continue
```

Or abort:

```bash
git cherry-pick --abort
```

# 🖆 Best Practices

* Inspect the source commit before cherry-picking it.
* Confirm that the destination branch is correct.
* Prefer small, self-contained commits.
* Check whether the selected commit depends on earlier changes.
* Use `-x` when tracking changes between branches is important.
* Resolve conflicts carefully and test the resulting code.
* Avoid cherry-picking the same changes repeatedly.
* Use merge or rebase when integrating an entire branch.
* Coordinate cherry-picks across release branches.
* Avoid cherry-picking merge commits unless you understand their parent structure.

# 🖆 Practical Exercises

* Create a repository with `main` and `feature` branches.
* Create several commits on the feature branch.
* Cherry-pick one commit into `main`.
* Verify that the destination commit has a different hash.
* Cherry-pick multiple commits in a single command.
* Cherry-pick a commit range.
* Use `--no-commit` and create a combined commit.
* Use `-x` to record the original commit reference.
* Create a merge commit and cherry-pick it using `-m 1`.
* Create a conflict and resolve it.
* Continue a cherry-pick using `--continue`.
* Abort a cherry-pick using `--abort`.
* Apply the same hotfix to two release branches.

# 🖆 Interview Questions

**What is Git cherry-pick?**

A Git command used to apply the changes introduced by a specific commit to the current branch.

**Does cherry-pick copy the original commit?**

No. It creates a new commit containing the selected changes. The new commit normally has a different hash.

**What is the difference between cherry-pick and merge?**

Cherry-pick applies selected commits individually. Merge integrates branch histories and the changes introduced by the merged branch.

**Can you cherry-pick multiple commits?**

Yes.

```bash
git cherry-pick <commit-1> <commit-2>
```

**How do you cherry-pick a commit range?**

```bash
git cherry-pick A..B
```

This selects commits reachable from `B` but not from `A`.

**What does `--no-commit` do?**

It applies the changes without automatically creating a commit.

**What does the `-x` option do?**

It adds the original commit hash to the new commit message.

**How do you cherry-pick a merge commit?**

```bash
git cherry-pick -m 1 <merge-commit-hash>
```

**How do you resolve cherry-pick conflicts?**

Resolve the files, stage them, and continue:

```bash
git add .
git cherry-pick --continue
```

**How do you cancel a cherry-pick operation?**

```bash
git cherry-pick --abort
```

**Can you cherry-pick a commit from another branch without switching to it?**

Yes. If the commit is available locally, you can switch to the destination branch and cherry-pick its hash directly.

**When should you use cherry-pick instead of merge?**

When you need selected commits from another branch rather than the entire branch's changes.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [Git Documentation — git-cherry-pick](https://git-scm.com/docs/git-cherry-pick)
* [Git Documentation — git-merge](https://git-scm.com/docs/git-merge)
* [Git Documentation — git-rebase](https://git-scm.com/docs/git-rebase)
* [Git Documentation — git-log](https://git-scm.com/docs/git-log)
* [Pro Git Book — Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
