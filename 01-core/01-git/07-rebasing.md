# Git Rebasing

Git rebase is a command used to move or replay commits from one branch onto another base commit.

It is commonly used to:

* Keep feature branches updated with the latest changes from `main`.
* Create a cleaner, more linear commit history.
* Reorganize commits before submitting a pull request.
* Edit, combine, reorder, or remove commits.
* Maintain a readable project history.

Rebase is one of Git's most powerful history-management features.

However, unlike merging, rebasing rewrites commit history by creating new commits with different commit hashes.

# 🗁 Understanding Git Rebase

Suppose you have two branches:

* `main`: Contains the latest application code.
* `feature/login`: Contains your login feature development.

While you are working on the feature, other developers add new commits to `main`.

You want to integrate those updates into your feature branch.

You can use merge:

```bash
git switch feature/login
git merge main
```

Or rebase:

```bash
git switch feature/login
git rebase main
```

With rebase, Git replays your feature commits on top of the latest `main` commit.

This makes the feature branch history appear as though development started from the updated `main` branch.

# 🗁 Why Rebase Is Needed

Rebase is useful in several development workflows.

**Keep feature branches up to date**

```bash
git switch feature/login
git fetch origin
git rebase origin/main
```

**Create a linear commit history**

Instead of adding merge commits for every integration, rebase can produce a straight-line history.

**Clean up commits before a pull request**

You can:

* Combine temporary commits.
* Correct commit messages.
* Remove unnecessary commits.
* Reorder related changes.
* Edit existing commits.

**Move commits to another base**

Rebase can replay selected commits onto a different branch or commit.

# 🗁 Understanding Rebase with Commit Graphs

Consider this repository history:

```text
        C---D  main
       /
A---B
     \
      E---F  feature/login
```

Here:

* `A` and `B` are common commits.
* `C` and `D` are new commits on `main`.
* `E` and `F` are feature commits.

The feature branch has not yet incorporated the latest changes from `main`.

### 🖆 Before Rebase

```text
        C---D  main
       /
A---B
     \
      E---F  feature/login
```

### 🖆 Execute Rebase

```bash
git switch feature/login
git rebase main
```

Git replays the changes introduced by `E` and `F` on top of `D`.

### 🖆 After Rebase

```text
A---B---C---D---E'---F'  feature/login
             ^
             main
```

The commits `E'` and `F'` are new commits.

They may contain equivalent changes to `E` and `F`, but their commit hashes are different.

The original commits remain part of the repository's object database until they become unreachable and are eventually pruned.

# 🗁 How Git Rebase Works

A simplified explanation of the rebase process:

* Git identifies the commits that belong to the current branch but not the target base.
* It identifies the new base commit.
* It replays the selected commits one by one.
* Each replay creates a new commit.
* If a conflict occurs, Git pauses and asks you to resolve it.
* After all commits are replayed, the branch points to the new history.

For example:

```bash
git switch feature/login
git rebase main
```

Conceptually:

```text
Original commits:
E---F

New base:
A---B---C---D

Rebased history:
A---B---C---D---E'---F'
```

Rebase does not simply move existing commits. It recreates commits with new parent relationships.

# 🗁 Basic Rebase Workflow

A typical feature branch rebase workflow:

### 🖆 Check the Current Branch

```bash
git branch --show-current
```

### 🖆 Check the Working Tree

```bash
git status
```

Commit or stash uncommitted changes before rebasing.

### 🖆 Fetch Remote Updates

```bash
git fetch origin
```

This updates your remote-tracking branches.

### 🖆 Switch to the Feature Branch

```bash
git switch feature/login
```

### 🖆 Rebase onto the Updated Main Branch

```bash
git rebase origin/main
```

Git replays your feature commits on top of the latest fetched `main` history.

### 🖆 Resolve Conflicts if Necessary

If Git reports conflicts:

```bash
git status
```

Edit the conflicted files and resolve the differences.

Stage the resolved files:

```bash
git add <file>
```

Continue:

```bash
git rebase --continue
```

Repeat until the rebase completes.

### 🖆 Verify the Result

```bash
git log --oneline --graph --all
git status
```

# 🗁 The `git rebase` Command

Basic syntax:

```bash
git rebase <upstream>
```

Rebase the current branch onto another branch:

```bash
git rebase main
```

Rebase a specific branch:

```bash
git switch feature/login
git rebase main
```

Rebase onto a specific commit:

```bash
git rebase <commit-hash>
```

Rebase onto a remote-tracking branch:

```bash
git fetch origin
git rebase origin/main
```

Specify both the upstream and the branch to rebase:

```bash
git rebase main feature/login
```

This rebases `feature/login` onto `main` without requiring you to switch to `feature/login` first.

# 🗁 Rebase Options

Git provides several options to control the rebase process.

### 🖆 `--continue`

Continue the rebase after resolving conflicts.

```bash
git rebase --continue
```

### 🖆 `--abort`

Cancel the rebase and attempt to restore the branch to its original state.

```bash
git rebase --abort
```

### 🖆 `--skip`

Skip the current commit being replayed.

```bash
git rebase --skip
```

Use this only when you intentionally want to omit that commit's changes.

### 🖆 `--interactive`

Start an interactive rebase.

```bash
git rebase -i HEAD~3
```

This allows you to edit, reorder, combine, or remove commits.

### 🖆 `--onto`

Rebase a selected range of commits onto a different base.

```bash
git rebase --onto <new-base> <old-base> <branch>
```

This is particularly useful when moving feature work between branches.

### 🖆 `--autosquash`

Automatically arrange commits marked with `fixup!` or `squash!` next to their target commits during an interactive rebase.

```bash
git rebase -i --autosquash HEAD~5
```

### 🖆 `--autostash`

Automatically stash local modifications before rebasing and attempt to reapply them afterward.

```bash
git rebase --autostash main
```

If reapplying the changes causes conflicts, resolve them normally.

### 🖆 `--rebase-merges`

Attempt to recreate merge commits and branching structure during a rebase.

```bash
git rebase --rebase-merges main
```

By default, a normal rebase generally linearizes the selected commits rather than preserving the original merge topology.

# 🗁 Interactive Rebase

Interactive rebase allows you to modify a sequence of commits before or during replay.

It is commonly used to clean up commit history before sharing work.

Basic command:

```bash
git rebase -i HEAD~3
```

This opens an editor containing the last three commits that are selected for rebase.

Example:

```text
pick a123abc Add login page
pick b234bcd Add login validation
pick c345cde Fix login styling
```

Each line represents a commit.

The commands at the beginning of each line determine what Git does with that commit.

# 🗁 Interactive Rebase Commands

| Command | Meaning |
| --- | --- |
| `pick` | Keep the commit |
| `reword` | Keep the commit but edit its message |
| `edit` | Stop and allow the commit to be modified |
| `squash` | Combine the commit with the previous commit and edit the resulting message |
| `fixup` | Combine the commit with the previous commit and discard its message |
| `drop` | Remove the commit |
| `exec` | Run a shell command during the rebase |
| `break` | Stop the rebase at this point |
| `label` | Assign a label to the current position |
| `reset` | Move the current position to a previously defined label |
| `merge` | Recreate a merge commit when using rebase features that preserve merge structure |

The commands are listed in the interactive rebase todo file.

# 🗁 Squashing Commits

Squashing combines multiple commits into one commit.

Suppose your history looks like this:

```text
A---B---C---D---E  feature/login
```

You want to combine `C`, `D`, and `E` into one commit.

Start an interactive rebase:

```bash
git rebase -i HEAD~3
```

The editor might show:

```text
pick c345cde Add login page
squash d456def Add login validation
squash e567efg Fix login styling
```

Save and close the editor.

Git combines the commits and allows you to edit the final commit message.

Result:

```text
A---B---F  feature/login
```

Here, `F` represents the combined commit.

### 🖆 Difference Between `squash` and `fixup`

**Squash**

```text
pick abc123 Add login page
squash def456 Add validation
```

Combines the commits and allows you to edit the resulting commit message.

**Fixup**

```text
pick abc123 Add login page
fixup def456 Fix validation
```

Combines the commits but discards the fixup commit's message.

# 🗁 Reordering Commits

Interactive rebase allows you to change commit order.

Original history:

```text
A---B---C---D---E
```

Interactive rebase:

```bash
git rebase -i HEAD~3
```

Original todo list:

```text
pick abc123 Add login page
pick def456 Add API integration
pick ghi789 Add validation
```

Reorder it:

```text
pick abc123 Add login page
pick ghi789 Add validation
pick def456 Add API integration
```

Git replays the commits in the new order.

Be careful when reordering commits that depend on changes introduced by earlier commits. Reordering may cause conflicts or produce incorrect intermediate states.

# 🗁 Editing Commit Messages

To change a commit message:

```bash
git rebase -i HEAD~3
```

Change `pick` to `reword`:

```text
pick abc123 Add login page
reword def456 Update stuff
pick ghi789 Add validation
```

Save and close the editor.

Git opens another editor so you can enter the new commit message.

Example:

```text
Update login API integration
```

The commit is recreated with the new message and a new hash.

# 🗁 Editing Existing Commits

To modify the contents of an existing commit, use `edit`.

```bash
git rebase -i HEAD~3
```

Example todo list:

```text
pick abc123 Add login page
edit def456 Add login validation
pick ghi789 Add API integration
```

Git pauses at the selected commit.

Modify the files:

```bash
# Make the required changes
```

Stage them:

```bash
git add .
```

Amend the current commit:

```bash
git commit --amend
```

Continue the rebase:

```bash
git rebase --continue
```

Git replays the remaining commits.

# 🗁 Dropping Commits

To remove a commit from the rebased history, use `drop`.

```bash
git rebase -i HEAD~3
```

Example:

```text
pick abc123 Add login page
drop def456 Add temporary debug output
pick ghi789 Add API integration
```

Git omits the selected commit while replaying the sequence.

You can also delete the corresponding line from the todo list.

**Important:** Dropping a commit can remove important changes. Review dependent commits before doing so.

# 🗁 Splitting Commits

Splitting a commit means dividing one commit into multiple smaller commits.

Start an interactive rebase:

```bash
git rebase -i HEAD~3
```

Mark the target commit as `edit`:

```text
pick abc123 Add login page
edit def456 Add login page and validation
pick ghi789 Add API integration
```

When Git pauses, reset the commit while keeping the changes in your working tree:

```bash
git reset HEAD^
```

Now the changes are unstaged.

Stage only the first group of changes:

```bash
git add login.html login.css
```

Create the first commit:

```bash
git commit -m "Add login page"
```

Stage the remaining changes:

```bash
git add login-validation.js
```

Create the second commit:

```bash
git commit -m "Add login validation"
```

Continue:

```bash
git rebase --continue
```

The original commit has now been replaced by two separate commits.

# 🗁 Rebase Conflicts

A rebase conflict occurs when Git cannot automatically replay a commit onto the new base.

Example:

```text
CONFLICT (content): Merge conflict in app.js
error: could not apply abc123... Update application logic
```

Unlike a normal merge, rebase applies commits sequentially.

A conflict can occur while replaying any individual commit.

### 🖆 Understanding `ours` and `theirs` During Rebase

The meaning of `ours` and `theirs` can be confusing during a rebase.

During a rebase:

* `ours` generally refers to the new base plus commits already replayed.
* `theirs` refers to the commit currently being replayed.

This differs from the intuitive interpretation people often use during a normal merge.

Inspect the conflict carefully before choosing either version.

# 🗁 Resolving Rebase Conflicts

### 🖆 Check the Rebase Status

```bash
git status
```

### 🖆 Identify Conflicted Files

```bash
git diff --name-only --diff-filter=U
```

### 🖆 Open and Resolve the Files

Example conflict:

```javascript
<<<<<<< HEAD
const message = "Updated main version";
=======
const message = "Feature branch version";
>>>>>>> abc123
```

Edit the file to contain the desired final result:

```javascript
const message = "Combined application message";
```

Remove the conflict markers.

### 🖆 Stage Resolved Files

```bash
git add app.js
```

### 🖆 Continue the Rebase

```bash
git rebase --continue
```

If another conflict occurs, repeat the process.

### 🖆 Verify the Result

```bash
git status
git log --oneline --graph
```

# 🗁 Aborting and Skipping a Rebase

### 🖆 Abort the Entire Rebase

```bash
git rebase --abort
```

This attempts to restore the branch to its state before the rebase began.

### 🖆 Skip the Current Commit

```bash
git rebase --skip
```

This omits the commit currently being replayed.

Use this only when the commit is intentionally unnecessary. For example, its changes may already exist in the new base.

### 🖆 Continue After Resolving Conflicts

```bash
git add <resolved-file>
git rebase --continue
```

### 🖆 Check Whether a Rebase Is in Progress

```bash
git status
```

Git normally reports when a rebase is in progress and provides guidance for continuing or aborting.

# 🗁 Rebase onto a Different Base

The `--onto` option provides more precise control over which commits are moved and where they are replayed.

Syntax:

```bash
git rebase --onto <new-base> <old-base> <branch>
```

Consider this history:

```text
          D---E  feature-A
         /
A---B---C  main
     \
      F---G  feature-B
```

Suppose `feature-B` was accidentally created from `feature-A`, but it should have been based on `main`.

You want to move only the commits belonging to `feature-B` onto `main`.

Execute:

```bash
git rebase --onto main feature-A feature-B
```

Conceptually, Git takes the commits in `feature-B` that are not reachable from `feature-A` and replays them onto `main`.

Result:

```text
      D---E  feature-A
     /
A---B---C---F'---G'  feature-B
```

This is useful for correcting branch dependencies.

# 🗁 Rebase vs. Merge

Rebase and merge are both used to integrate changes, but their behavior is different.

### 🖆 Merge Example

```bash
git switch feature/login
git merge main
```

Possible result:

```text
        C---D
       /     \
A---B         M  feature/login
     \       /
      E---F
```

Merge preserves the original branch structure.

### 🖆 Rebase Example

```bash
git switch feature/login
git rebase main
```

Result:

```text
A---B---C---D---E'---F'  feature/login
```

Rebase creates a linear history by replaying the feature commits.

### 🖆 Comparison

| Feature                          | Merge                   | Rebase                                             |
| -------------------------------- | ----------------------- | -------------------------------------------------- |
| Rewrites commit history          | No                      | Yes                                                |
| Creates merge commits            | Sometimes               | No, for ordinary rebase                            |
| Preserves original commit hashes | Yes                     | No, for replayed commits                           |
| Produces linear history          | Not necessarily         | Usually                                            |
| Preserves merge topology         | Yes                     | Requires `--rebase-merges` to attempt preservation |
| Best suited for                  | Integrating shared work | Updating and cleaning private feature branches     |

Neither approach is universally better. Choose based on project workflow and team conventions.

# 🗁 Rebasing Shared and Public Branches

Rebasing changes commit identities.

Suppose a shared branch contains:

```text
A---B---C---D  shared-feature
```

After rebasing, the commits may become:

```text
A---B---C'---D'  shared-feature
```

The original commits `C` and `D` have been replaced by new commits.

Other developers may still have the original history.

This can cause problems when they attempt to pull or push changes.

### 🖆 Why Rewriting Shared History Is Dangerous

* Existing commit references become outdated.
* Other developers may have to reconcile divergent histories.
* Previously based branches may need to be rebased again.
* Force pushing can overwrite remote branch updates.

### 🖆 Safer Alternative

For shared branches, merging is often safer:

```bash
git switch shared-feature
git merge main
```

### 🖆 If Rewriting a Remote Feature Branch Is Necessary

After rebasing a branch that you own, you may need to update its remote counterpart.

Use:

```bash
git push --force-with-lease origin feature/login
```

`--force-with-lease` checks that the remote branch still points to the expected commit before updating it.

It is safer than:

```bash
git push --force
```

However, it does not make rewriting shared history harmless. Coordinate with collaborators before doing so.

# 🗁 Recovering from a Rebase

Git provides recovery mechanisms if a rebase produces an unexpected result.

### 🖆 Inspect the Reflog

```bash
git reflog
```

The reflog records recent updates to local references, including many operations that change branch positions.

Example:

```text
abc123 HEAD@{0}: rebase (finish): returning to refs/heads/feature/login
def456 HEAD@{1}: rebase (pick): Add validation
ghi789 HEAD@{2}: checkout: moving from main to feature/login
```

### 🖆 Identify the Previous Commit

Find the branch state before the rebase began.

### 🖆 Restore the Previous State

If the correct previous commit is `ghi789`:

```bash
git reset --hard ghi789
```

This restores the current branch and working tree to that commit.

**Important:** A hard reset can discard uncommitted work. Preserve any valuable changes before using it.

# 🗁 Common Problems and Troubleshooting

### 🖆 Cannot Rebase: Uncommitted Changes

Git may refuse to start a rebase because local modifications could be overwritten.

Check:

```bash
git status
```

Commit the changes:

```bash
git add .
git commit -m "Save current work"
```

Or stash them:

```bash
git stash
git rebase main
git stash pop
```

### 🖆 Rebase Conflict

Message:

```text
error: could not apply abc123... Update application logic
```

Solution:

```bash
git status
```

Resolve the conflict, stage the file, and continue:

```bash
git add <file>
git rebase --continue
```

### 🖆 Rebase Appears Stuck

Check:

```bash
git status
```

If a conflict is in progress, resolve it and continue:

```bash
git rebase --continue
```

To cancel:

```bash
git rebase --abort
```

### 🖆 Empty Commit During Rebase

A commit may become empty because its changes are already present in the new base.

Git may report that the commit is empty.

Review the situation. If the commit is genuinely unnecessary, you can skip it:

```bash
git rebase --skip
```

Do not skip automatically without understanding what the commit was intended to change.

### 🖆 Push Rejected After Rebase

Message:

```text
! [rejected] feature/login -> feature/login (non-fast-forward)
```

This can happen because the remote branch still contains the old commit history.

If you own the branch and have coordinated the rewrite:

```bash
git push --force-with-lease origin feature/login
```

If the branch is shared, coordinate with collaborators before replacing remote history.

### 🖆 Duplicate Commits After Rebase

This can happen when branches contain similar changes or when commits are replayed from an unexpected base.

Inspect the history:

```bash
git log --graph --oneline --all
```

Inspect the branch relationship:

```bash
git merge-base main feature/login
```

Review the commit range before attempting another rebase.

# 🖆 Best Practices

* Use rebase to update private feature branches.
* Prefer merge when integrating work into shared branches unless your team has a different established policy.
* Understand that rebase creates new commit hashes.
* Avoid rebasing commits that other developers have already based work on.
* Clean up temporary commits before opening a pull request.
* Use interactive rebase to improve commit organization.
* Keep individual commits focused and meaningful.
* Review the interactive rebase todo list before saving it.
* Use `--force-with-lease` rather than plain `--force` when updating a rewritten remote branch.
* Resolve conflicts carefully and run tests after rebasing.
* Use `git reflog` to locate previous branch states when recovery is necessary.
* Do not use `git rebase --skip` unless you intentionally want to omit the current commit.

# 🖆 Practical Exercises

### 🖆 Exercise: Basic Rebase

Create a repository:

```bash
mkdir rebase-practice
cd rebase-practice
git init
```

Create the initial commit:

```bash
echo "# Rebase Practice" > README.md
git add README.md
git commit -m "Initial commit"
```

Create a feature branch:

```bash
git switch -c feature/login
```

Add a feature commit:

```bash
echo "Login feature" > login.txt
git add login.txt
git commit -m "Add login feature"
```

Return to `main`:

```bash
git switch main
```

Add a separate commit:

```bash
echo "Main update" > main.txt
git add main.txt
git commit -m "Update main branch"
```

Rebase the feature branch:

```bash
git switch feature/login
git rebase main
```

Inspect the history:

```bash
git log --graph --oneline --all
```

### 🖆 Exercise: Interactive Rebase

Create several commits:

```bash
echo "Feature A" > feature-a.txt
git add feature-a.txt
git commit -m "Add feature A"

echo "Feature B" > feature-b.txt
git add feature-b.txt
git commit -m "Add feature B"

echo "Fix feature B" >> feature-b.txt
git add feature-b.txt
git commit -m "Fix feature B"
```

Start interactive rebase:

```bash
git rebase -i HEAD~3
```

Try these operations:

* Change a commit message using `reword`.
* Combine commits using `squash`.
* Remove a commit using `drop`.
* Reorder commits.

Inspect the resulting history:

```bash
git log --oneline --graph
```

### 🖆 Exercise: Resolve a Rebase Conflict

Create two branches with conflicting changes.

On `main`:

```bash
echo "Original content" > app.txt
git add app.txt
git commit -m "Add app file"
```

Create a feature branch:

```bash
git switch -c feature/update
```

Modify the file:

```bash
echo "Feature version" > app.txt
git add app.txt
git commit -m "Update app from feature"
```

Switch to `main`:

```bash
git switch main
```

Modify the same line:

```bash
echo "Main version" > app.txt
git add app.txt
git commit -m "Update app from main"
```

Rebase the feature branch:

```bash
git switch feature/update
git rebase main
```

Resolve the conflict, then:

```bash
git add app.txt
git rebase --continue
```

Verify:

```bash
git log --graph --oneline --all
```

### 🖆 Exercise: Recover Using Reflog

Before rebasing, inspect the reflog:

```bash
git reflog
```

Perform an interactive rebase and then inspect the reflog again:

```bash
git rebase -i HEAD~2
git reflog
```

Identify the branch position before the rebase.

In a disposable test repository, practice restoring that state:

```bash
git reset --hard <previous-commit-hash>
```

# 🖆 Interview Questions

**What is Git rebase?**

Git rebase replays commits from one branch onto another base commit, creating new commits with updated parent relationships.

**Why is rebase used?**

It is used to update feature branches, maintain linear history, and clean up commits before integration.

**Does rebase change commit hashes?**

Yes. Replayed commits receive new hashes because their commit metadata and parent relationships change.

**What is interactive rebase?**

Interactive rebase allows developers to edit, reorder, squash, fix up, or drop commits.

**What is the difference between `squash` and `fixup`?**

Both combine commits. `squash` allows editing the combined commit message, while `fixup` discards the message of the commit being combined.

**What does `git rebase --continue` do?**

It resumes an in-progress rebase after conflicts have been resolved and staged.

**What does `git rebase --abort` do?**

It cancels an in-progress rebase and attempts to restore the original branch state.

**What does `git rebase --skip` do?**

It skips the commit currently being replayed.

**Why should shared branches generally not be rebased?**

Rebase rewrites commit history, which can disrupt collaborators who already depend on the original commits.

**What is the purpose of `git rebase --onto`?**

It allows selected commits to be replayed onto a different base.

**What is the difference between merge and rebase?**

Merge combines branch histories and may create a merge commit. Rebase replays commits onto a new base, creating new commit hashes.

**How can you recover from an accidental rebase?**

Use `git reflog` to locate the previous branch position and restore it if necessary.

**What is `--force-with-lease`?**

It updates a remote branch only if the remote reference still matches the expected state, providing a safer alternative to plain force pushing.

**What is `--rebase-merges`?**

It instructs Git to attempt to recreate merge commits and preserve branching structure during a rebase.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [Git Documentation — git-rebase](https://git-scm.com/docs/git-rebase)
* [Git Documentation — git-reflog](https://git-scm.com/docs/git-reflog)
* [Git Documentation — git-merge-base](https://git-scm.com/docs/git-merge-base)
* [Pro Git Book — Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
* [Pro Git Book — Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
