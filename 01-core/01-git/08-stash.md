# Git Stash

Git stash is a feature that temporarily saves changes in your working directory so you can work on something else without creating a commit.

It is especially useful when you are working on a feature and suddenly need to switch branches or handle an urgent bug fix.

For example:

* You are implementing a login feature.
* Your changes are incomplete.
* An urgent bug needs to be fixed on another branch.
* You do not want to commit unfinished work.
* You stash your changes, switch branches, and handle the bug.
* Later, you restore your stashed changes and continue development.

Git stash provides a convenient way to temporarily set aside work.

# 🗁 Understanding Git Stash

Git stash saves selected local modifications and restores the working tree to a clean state.

By default, it saves:

* Changes to tracked files that are already staged.
* Changes to tracked files that are not staged.

By default, it does not save:

* Untracked files.
* Ignored files.

You can include these additional files using specific options.

Example:

```bash
git stash
```

This saves tracked modifications and removes them from the working tree.

Later, restore the most recent stash:

```bash
git stash pop
```

The changes are reapplied to the working tree, and the stash entry is removed if the application succeeds.

# 🗁 Why Git Stash Is Needed

Stash is useful when you need to temporarily move away from your current work.

Common situations include:

* Switching branches without committing unfinished changes.
* Handling urgent production bugs.
* Testing another branch.
* Pulling updates when local modifications would interfere.
* Temporarily cleaning the working directory.
* Saving work before experimenting with another implementation.
* Separating unrelated changes into different tasks.

Example:

```text
feature/login
    |
    |--- Working on login form
    |
    |--- Urgent bug reported
    |
    |--- Stash current changes
    |
    |--- Switch to main
    |
    |--- Fix urgent bug
    |
    |--- Return to feature/login
    |
    |--- Restore stashed changes
```

# 🗁 How Git Stash Works

Git stash creates internal Git objects representing the saved state of your work.

A typical stash entry records:

* The original `HEAD` commit.
* The index state, representing staged changes.
* The working tree state.
* An additional parent for untracked files when using `--include-untracked` or `--all`, if such files are present.

Conceptually:

```text
              W  Working tree changes
             /
        S---+
       /    \
      H      I
```

Where:

* `H` represents the original `HEAD`.
* `I` represents the saved index state.
* `W` represents the saved working tree state.
* `S` represents the stash commit.

The stash is stored in a reference commonly named:

```text
refs/stash
```

The stash list provides a convenient way to access saved entries.

You do not need to understand the internal structure to use stash commands effectively, but knowing that stash preserves both staged and unstaged changes helps explain its behavior.

# 🗁 Basic Stash Workflow

Consider that you are working on a feature branch.

Check your current branch:

```bash
git branch --show-current
```

Check your working tree:

```bash
git status
```

Save your changes:

```bash
git stash
```

Switch to another branch:

```bash
git switch main
```

Complete the urgent task.

Return to your feature branch:

```bash
git switch feature/login
```

Restore your changes:

```bash
git stash pop
```

Verify the result:

```bash
git status
```

Your previously saved changes are now back in your working tree.

# 🗁 The `git stash` Command

Basic syntax:

```bash
git stash [command] [options]
```

Common commands:

| Command | Purpose |
| --- | --- |
| `git stash` | Save tracked changes |
| `git stash push` | Save changes explicitly |
| `git stash list` | List saved stashes |
| `git stash show` | Show a stash summary |
| `git stash apply` | Apply a stash without removing it |
| `git stash pop` | Apply and remove a stash if successful |
| `git stash drop` | Delete a stash entry |
| `git stash clear` | Remove all stash entries |
| `git stash branch` | Create a branch from a stash |
| `git stash push -p` | Interactively select changes to stash |

# 🗁 Stashing Changes

### 🖆 Save Tracked Changes

```bash
git stash
```

This is equivalent to:

```bash
git stash push
```

Git saves tracked staged and unstaged changes.

### 🖆 Save Changes with a Message

```bash
git stash push -m "Work on login feature"
```

A descriptive message makes it easier to identify the stash later.

### 🖆 Stash Only Tracked Files

```bash
git stash push --keep-index
```

This saves changes that are not staged while leaving the staged changes in the working tree and index.

Use this when you want to temporarily set aside unstaged work but preserve the staged changes.

### 🖆 Keep Staged Changes in the Index

The `--staged` option stashes only the changes currently staged in the index:

```bash
git stash push --staged
```

This is useful when you want to temporarily set aside staged work without stashing your other tracked working-tree modifications.

### 🖆 Interactively Stash Selected Changes

```bash
git stash push --patch
```

Git presents individual change hunks and allows you to choose which ones to stash.

Common interactive choices include:

| Choice | Meaning |
| --- | --- |
| `y` | Stash this hunk |
| `n` | Leave this hunk unchanged |
| `q` | Quit |
| `a` | Stash this and all remaining hunks in the file |
| `d` | Do not stash this or remaining hunks in the file |
| `s` | Split the current hunk into smaller hunks when possible |
| `e` | Manually edit the hunk |

This is useful when a single file contains changes belonging to multiple tasks.

# 🗁 Viewing Stashes

List all saved stash entries:

```bash
git stash list
```

Example:

```text
stash@{0}: On feature/login: Work on login form
stash@{1}: On main: Update application configuration
stash@{2}: On feature/payment: Payment validation changes
```

Each entry has a reference:

* `stash@{0}` is the most recent stash.
* `stash@{1}` is the previous stash.
* `stash@{2}` is an older stash.

These indexes can change when stash entries are added or removed.

### 🖆 View the Latest Stash Summary

```bash
git stash show
```

### 🖆 View a Specific Stash Summary

```bash
git stash show stash@{1}
```

### 🖆 Display the Full Diff

```bash
git stash show -p
```

For a specific stash:

```bash
git stash show -p stash@{1}
```

### 🖆 Show Staged and Unstaged Changes Separately

A stash can preserve both the staged index state and unstaged working-tree changes.

To inspect the differences:

```bash
git diff stash@{0}^1 stash@{0}^2
```

This shows the staged changes represented by the stash's index commit.

To inspect the additional unstaged changes:

```bash
git diff stash@{0}^2 stash@{0}
```

The exact internal structure can vary when untracked files are included, but the first two parents represent the original base and index state in the usual stash structure.

# 🗁 Applying Stashes

The `apply` command restores a stash without deleting it.

```bash
git stash apply
```

This applies the latest stash.

Apply a specific stash:

```bash
git stash apply stash@{1}
```

Apply a stash and attempt to restore its original index state:

```bash
git stash apply --index
```

Without `--index`, the saved changes are generally applied as working-tree modifications rather than restoring their original staged state.

The stash entry remains available after applying.

Example:

```bash
git stash apply stash@{0}
```

You can apply the same stash again later if needed, although applying it repeatedly may produce conflicts or duplicate changes.

# 🗁 Popping Stashes

The `pop` command applies a stash and removes its entry if the application succeeds.

```bash
git stash pop
```

Apply a specific stash:

```bash
git stash pop stash@{1}
```

Restore the original index state when possible:

```bash
git stash pop --index
```

### 🖆 Difference Between `apply` and `pop`

| Feature                         | `apply` | `pop`                                                |
| ------------------------------- | ------- | ---------------------------------------------------- |
| Restores changes                | Yes     | Yes                                                  |
| Removes stash entry             | No      | Yes, if application succeeds                         |
| Useful for repeated restoration | Yes     | Not by default                                       |
| Safer when keeping a backup     | Yes     | No, because successful application removes the entry |

**Important:** If `git stash pop` causes conflicts, Git keeps the stash entry instead of dropping it. Resolve the conflicts and verify the result before deleting the stash manually.

# 🗁 Managing Multiple Stashes

Git supports multiple stash entries.

Example:

```bash
git stash push -m "Login feature changes"
git stash push -m "Payment feature changes"
git stash push -m "Configuration changes"
```

List them:

```bash
git stash list
```

Example:

```text
stash@{0}: On main: Configuration changes
stash@{1}: On feature/payment: Payment feature changes
stash@{2}: On feature/login: Login feature changes
```

### 🖆 Apply an Older Stash

```bash
git stash apply stash@{2}
```

### 🖆 Drop a Specific Stash

```bash
git stash drop stash@{1}
```

### 🖆 Apply the Latest Stash

```bash
git stash apply stash@{0}
```

### 🖆 💡IMPORTANT: Stash Indexes Are Not Permanent Identifiers

When you add or remove stash entries, the numerical indexes can change.

For example, after dropping `stash@{0}`, the previous `stash@{1}` may become `stash@{0}`.

Always check the stash list before operating on a particular entry.

# 🗁 Stashing Untracked Files

By default, Git stash does not include untracked files.

Example:

```bash
echo "Temporary work" > temporary.txt
```

Check the status:

```bash
git status
```

The file appears as untracked.

Running:

```bash
git stash
```

does not save `temporary.txt`.

To include untracked files:

```bash
git stash push --include-untracked
```

Short form:

```bash
git stash push -u
```

This saves tracked modifications and untracked files.

### 🖆 Restore the Stash

```bash
git stash pop
```

The untracked file is restored along with the other saved changes.

# 🗁 Stashing Ignored Files

Ignored files are excluded from normal stash operations.

To include both untracked and ignored files:

```bash
git stash push --all
```

Short form:

```bash
git stash push -a
```

This includes:

* Tracked modifications.
* Untracked files.
* Ignored files.

Example:

```bash
git stash push -a -m "Save all local files"
```

Use this carefully because ignored files may contain build output, local configuration, caches, or generated data that you do not normally want to stash.

# 🗁 Creating Named Stashes

Use descriptive messages to make stash entries easier to identify.

```bash
git stash push -m "Implement login validation"
```

Another example:

```bash
git stash push -u -m "Save unfinished API integration"
```

View the descriptions:

```bash
git stash list
```

Example:

```text
stash@{0}: On feature/api: Save unfinished API integration
stash@{1}: On feature/login: Implement login validation
```

A good stash message should describe the work rather than simply saying "temporary changes."

# 🗁 Inspecting Stash Contents

### 🖆 Show a Summary

```bash
git stash show stash@{0}
```

### 🖆 Show the Full Patch

```bash
git stash show -p stash@{0}
```

### 🖆 Show Changed File Names

```bash
git diff-tree --no-commit-id --name-only -r stash@{0}
```

### 🖆 Compare a Stash with Its Original Base

```bash
git diff stash@{0}^1 stash@{0}
```

This displays the overall tracked changes represented by the stash's working-tree snapshot relative to its original base.

# 🗁 Creating a Branch from a Stash

Sometimes you stash changes on one branch but later decide that they belong in a different branch.

Git provides:

```bash
git stash branch <new-branch-name> <stash>
```

Example:

```bash
git stash branch feature/new-login stash@{0}
```

Git:

* Creates a new branch at the commit where the stash was originally created.
* Applies the stash to that branch.
* Drops the stash entry if application succeeds.

This is particularly useful when a stash cannot be applied cleanly to the current branch because the surrounding history has changed.

Example workflow:

```bash
git stash list
git stash branch feature/new-login stash@{0}
```

Verify:

```bash
git status
git branch --show-current
```

# 🗁 Deleting Stashes

### 🖆 Delete a Specific Stash

```bash
git stash drop stash@{0}
```

### 🖆 Delete the Latest Stash

```bash
git stash drop
```

### 🖆 Delete All Stashes

```bash
git stash clear
```

**Important:** `git stash clear` removes all stash references. Recovering cleared stash entries can be difficult, so inspect your stash list before using it.

# 🗁 Stash Conflicts

A stash conflict occurs when Git cannot cleanly apply saved changes to the current working tree.

For example:

* You stash a change to a particular line.
* Another branch modifies the same line.
* You apply the stash on that branch.
* Git cannot automatically combine the changes.

Git reports a conflict and leaves the affected files for manual resolution.

### 🖆 Resolve a Stash Conflict

Check the status:

```bash
git status
```

Open the conflicted files.

You may see conflict markers:

```text
<<<<<<< Updated upstream
Current branch content
=======
Stashed content
>>>>>>> Stashed changes
```

Edit the file and choose the desired final content.

Stage the resolved file:

```bash
git add <file>
```

Check the result:

```bash
git status
```

Unlike a merge or rebase, applying a stash does not have a dedicated `--continue` operation.

After resolving conflicts, the changes are part of your working tree. Continue working or commit them normally.

If you used `git stash pop` and a conflict occurred, the stash entry is normally retained. Verify that your changes have been restored correctly before dropping it.

# 🗁 Stash Internals

A stash is stored as Git commit objects, not as a simple temporary text file.

Inspect the latest stash:

```bash
git log --graph --oneline --all --reflog
```

Inspect its commit details:

```bash
git show --summary stash@{0}
```

Display the stash reference:

```bash
git rev-parse refs/stash
```

A typical stash structure includes:

* A stash commit representing the saved working-tree state.
* A first parent representing the original `HEAD`.
* A second parent representing the index state.
* An additional parent when untracked files are included and saved.

The `refs/stash` reference points to the latest stash. Older entries are retained through its reflog.

This internal structure explains why stash can preserve staged and unstaged changes separately.

# 🗁 Stash vs. Commit

Both stash and commit preserve changes, but they serve different purposes.

| Feature | Stash | Commit |
| --- | --- | --- |
| Purpose | Temporarily save work | Permanently record work in branch history |
| Appears in normal branch history | No | Yes |
| Requires a commit message | Optional | Yes |
| Suitable for unfinished work | Yes | Yes, if using temporary commits |
| Shared with remote by default | No | Can be pushed |
| Easy to restore later | Yes | Yes, through normal Git operations |

Use stash when you need to temporarily set work aside.

Use commits when you want to create a durable, identifiable point in project history.

# 🗁 Stash vs. Temporary Branch

A temporary branch is another way to preserve unfinished work.

| Feature | Stash | Temporary branch |
| --- | --- | --- |
| Saves uncommitted work | Yes | Requires a commit or another saving method |
| Preserves work in branch history | No | Yes |
| Easy to switch between tasks | Yes | Yes |
| Supports multiple checkpoints | Multiple stash entries | Multiple commits |
| Suitable for long-running work | Less suitable | More suitable |
| Easy to share | No, not directly | Yes, by pushing the branch |

For short interruptions, stash is convenient.

For longer tasks, a temporary branch with meaningful commits is often easier to manage.

# 🗁 Recovering Lost Stashes

Stash entries are references to Git objects. If a stash is dropped or cleared, the objects may still exist temporarily until Git garbage collection removes unreachable objects.

Recovery is not guaranteed, but you can investigate.

### 🖆 Inspect the Reflog

```bash
git reflog show refs/stash
```

This displays the stash reference history while its entries remain available.

### 🖆 Search Unreachable Objects

```bash
git fsck --no-reflogs --unreachable
```

This can reveal unreachable commits and other objects.

### 🖆 Inspect a Candidate Commit

If you identify a possible stash commit:

```bash
git show <commit-hash>
```

Check whether its contents match your missing work.

### 🖆 Preserve a Recovered Stash

If the object is a valid stash commit, you can create a new reference:

```bash
git stash store -m "Recovered stash" <commit-hash>
```

Then verify:

```bash
git stash list
```

Recovery becomes harder after garbage collection prunes the unreachable objects. Avoid running cleanup operations while investigating lost stash data.

# 🗁 Common Problems and Troubleshooting

### 🖆 Untracked Files Were Not Stashed

**Cause:** Normal stash operations exclude untracked files.

**Solution:**

```bash
git stash push -u
```

### 🖆 Ignored Files Were Not Stashed

**Cause:** Ignored files are excluded by default.

**Solution:**

```bash
git stash push -a
```

### 🖆 Stash Apply Produces Conflicts

**Cause:** The current branch has changes that conflict with the saved changes.

**Solution:**

```bash
git status
```

Resolve the conflicts manually, then stage the resolved files:

```bash
git add <file>
```

### 🖆 Stash Pop Did Not Remove the Stash

**Cause:** Applying the stash resulted in conflicts or did not complete successfully.

**Solution:**

```bash
git stash list
```

Resolve the conflicts and verify the restored changes. Drop the stash manually only when you no longer need it:

```bash
git stash drop stash@{0}
```

### 🖆 Staged Changes Were Restored as Unstaged

**Cause:** The stash was applied without restoring its original index state.

**Solution:**

Use `--index` when applying a stash:

```bash
git stash apply --index
```

This attempts to restore the staged and unstaged states separately.

### 🖆 Cannot Switch Branch Because of Local Changes

**Cause:** The working tree contains modifications that would be overwritten by switching branches.

**Solution:**

```bash
git stash push -u -m "Save work before switching branches"
git switch main
```

### 🖆 Stash Entry Is Missing

**Cause:** The stash may have been dropped, cleared, or removed from the reference history.

**Solution:**

Inspect:

```bash
git reflog show refs/stash
```

If necessary, investigate unreachable objects:

```bash
git fsck --no-reflogs --unreachable
```

# 🖆 Best Practices

* Use descriptive stash messages.
* Run `git stash list` before applying or deleting a particular entry.
* Use `git stash push -u` when untracked files must be saved.
* Use `git stash push -a` only when ignored files also need to be included.
* Prefer `apply` when you want to preserve a backup stash entry.
* Use `pop` when you are confident the stash is no longer needed after successful restoration.
* Use `--index` when preserving the original staged state matters.
* Resolve stash conflicts carefully.
* Verify restored files before deleting stash entries.
* Avoid using `git stash clear` unless you intentionally want to remove every stash.
* Use a temporary branch for work that needs durable checkpoints or collaboration.
* Do not treat stash as a permanent backup mechanism.

# 🖆 Practical Exercises

### 🖆 Exercise: Basic Stash

Create a repository:

```bash
mkdir stash-practice
cd stash-practice
git init
```

Create an initial commit:

```bash
echo "# Stash Practice" > README.md
git add README.md
git commit -m "Initial commit"
```

Modify the file:

```bash
echo "New documentation" >> README.md
```

Stash the changes:

```bash
git stash
```

Check the working tree:

```bash
git status
```

Restore the changes:

```bash
git stash pop
```

### 🖆 Exercise: Multiple Stashes

Create multiple modifications:

```bash
echo "Login feature" > login.txt
git add login.txt
git commit -m "Add login file"
```

Modify the file:

```bash
echo "Login validation" >> login.txt
git stash push -m "Login validation changes"
```

Create another file:

```bash
echo "Payment feature" > payment.txt
git stash push -u -m "Payment feature changes"
```

List the stashes:

```bash
git stash list
```

Inspect a specific stash:

```bash
git stash show -p stash@{0}
```

Apply an older stash:

```bash
git stash apply stash@{1}
```

### 🖆 Exercise: Stash Untracked Files

Create an untracked file:

```bash
echo "Temporary file" > temporary.txt
```

Try:

```bash
git stash
git status
```

Observe that the untracked file remains.

Now stash untracked files:

```bash
git stash push -u -m "Save untracked file"
```

Check:

```bash
git status
git stash list
```

Restore:

```bash
git stash pop
```

### 🖆 Exercise: Apply vs. Pop

Create a change:

```bash
echo "Test stash behavior" >> README.md
git stash push -m "Apply vs pop"
```

Apply it:

```bash
git stash apply
```

Check:

```bash
git stash list
git status
```

Notice that the stash still exists.

Reset the test modification if necessary, then apply and remove the stash:

```bash
git stash pop
```

Observe the difference.

### 🖆 Exercise: Create a Branch from a Stash

Create and stash some work:

```bash
echo "New feature work" > new-feature.txt
git stash push -u -m "New feature work"
```

Create a branch from the stash:

```bash
git stash branch feature/new-work stash@{0}
```

Verify:

```bash
git branch --show-current
git status
git stash list
```

# 🖆 Interview Questions

**What is Git stash?**

Git stash temporarily saves local changes so that you can work on another task without creating a commit.

**Why is Git stash used?**

It is used when you need to switch branches, handle urgent work, or temporarily clean your working directory.

**What is the difference between `git stash apply` and `git stash pop`?**

`apply` restores changes while keeping the stash entry. `pop` restores changes and removes the stash entry if application succeeds.

**Does `git stash` save untracked files?**

No. Use `git stash push -u` to include untracked files.

**How do you stash ignored files?**

```bash
git stash push -a
```

**How do you list all stashes?**

```bash
git stash list
```

**How do you apply a specific stash?**

```bash
git stash apply stash@{1}
```

**How do you delete a specific stash?**

```bash
git stash drop stash@{1}
```

**How do you delete all stashes?**

```bash
git stash clear
```

**What is the difference between `--include-untracked` and `--all`?**

`--include-untracked` includes untracked files. `--all` also includes ignored files.

**How do you restore the original staged state of a stash?**

```bash
git stash apply --index
```

**What happens if `git stash pop` produces conflicts?**

Git leaves the stash entry in the stash list. Resolve the conflicts and verify the restored changes.

**Can you create a branch from a stash?**

Yes.

```bash
git stash branch feature/new-work stash@{0}
```

**Where does Git store stash references?**

The latest stash is referenced by `refs/stash`. Previous entries are tracked through its reflog.

**Can a dropped stash be recovered?**

Sometimes. You can inspect the stash reflog and search unreachable Git objects, provided they have not been pruned.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [Git Documentation — git-stash](https://git-scm.com/docs/git-stash)
* [Git Documentation — git-reset](https://git-scm.com/docs/git-reset)
* [Git Documentation — git-reflog](https://git-scm.com/docs/git-reflog)
* [Pro Git Book — Stashing and Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)
* [Git Documentation — git-fsck](https://git-scm.com/docs/git-fsck)
