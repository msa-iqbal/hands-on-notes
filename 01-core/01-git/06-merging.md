# Git Merging

Git merging is the process of combining changes from different branches into a single branch.

It allows developers to work independently on features, bug fixes, or improvements and later integrate their work into another branch.

For example:

* `main` contains the stable application code.
* `feature/login` contains a new login feature.
* After completing the feature, you merge `feature/login` into `main`.

Git attempts to combine the changes while preserving the development history.

# 🗁 Understanding Git Merging

Git stores project history as a graph of commits.

When two branches contain different commits, merging combines their histories and integrates their changes.

Example:

```text
        A---B---C  main
             \
              D---E  feature/login
```

After merging `feature/login` into `main`, the history might look like this:

```text
        A---B---C-------M  main
             \         /
              D---E---    feature/login
```

Here:

* `A`, `B`, and `C` are commits on `main`.
* `D` and `E` are commits on `feature/login`.
* `M` is the merge commit.

The merge commit records the integration of the two histories.

**Important:** The `git merge` command merges the specified branch into the current branch.

```bash
git switch main
git merge feature/login
```

This integrates `feature/login` into `main`, not the other way around.

# 🗁 Why Merging Is Needed

Merging is useful when multiple branches are used for independent development.

Common scenarios include:

* Integrating completed features into the main development branch.
* Combining bug fixes with application code.
* Integrating work from multiple developers.
* Merging release branches into the main branch.
* Bringing changes from an updated main branch into a feature branch.
* Combining independently developed work.

Example workflow:

```text
main
 |
 |---- feature/login
 |         |
 |         |--- Login implementation
 |         |--- Login validation
 |         |
 |--------- Merge feature/login
```

# 🗁 Merge Terminology

| Term            | Meaning                                                           |
| --------------- | ----------------------------------------------------------------- |
| Current branch  | The branch currently checked out                                  |
| Source branch   | The branch whose changes are being integrated                     |
| Target branch   | The branch receiving the changes                                  |
| Merge base      | The best common ancestor of two commits                           |
| Fast-forward    | Moving a branch reference forward without creating a merge commit |
| Three-way merge | Combining changes using two branch tips and their common ancestor |
| Merge commit    | A commit that records the integration of multiple histories       |
| Merge conflict  | A situation where Git cannot automatically combine changes        |

**Example:**

```bash
git switch main
git merge feature/login
```

* Current branch: `main`
* Source branch: `feature/login`
* Target branch: `main`

# 🗁 Understanding Merge Types

Git primarily performs two kinds of merges:

* Fast-forward merge
* Three-way merge

The type of merge depends on the relationship between the branch histories.

### 🖆 Fast-Forward Merge

A fast-forward merge happens when the current branch has not diverged from the branch being merged.

In other words, the current branch tip is an ancestor of the incoming branch tip.

Before merging:

```text
A---B  main
     \
      C---D  feature/login
```

The `main` branch has no new commits since `feature/login` branched off.

Execute:

```bash
git switch main
git merge feature/login
```

After merging:

```text
A---B---C---D  main, feature/login
```

Git simply moves the `main` branch reference forward.

No merge commit is created.

**Advantages:**

* Keeps history linear.
* Avoids unnecessary merge commits.
* Makes simple branch histories easier to read.

**Disadvantages:**

* Does not preserve a separate merge point.
* Feature branch boundaries become less visible in the commit history.

### 🖆# Force a Merge Commit

Use `--no-ff` to prevent fast-forward merging:

```bash
git merge --no-ff feature/login
```

This creates a merge commit even when fast-forwarding is possible.

Example:

```text
A---B-----------M  main
     \         /
      C---D---    feature/login
```

This makes the integration point visible in the history.

### 🖆 Three-Way Merge

A three-way merge happens when both branches have developed independently.

Example:

```text
        C---D  main
       /
A---B
       \
        E---F  feature/login
```

Both branches contain commits that are not present in the other branch.

Git identifies three points:

* Common ancestor: `B`
* Current branch tip: `D`
* Incoming branch tip: `F`

Git compares the changes made on both branches relative to the common ancestor and attempts to combine them.

Execute:

```bash
git switch main
git merge feature/login
```

After a successful merge:

```text
        C---D-------M  main
       /           /
A---B              /
       \           /
        E---F-----
```

The merge commit `M` records the integration.

If Git cannot automatically combine some changes, it reports a merge conflict.

# 🗁 Merge Commits

A merge commit is a commit that combines multiple lines of development.

A normal merge commit typically has two parents:

* First parent: the previous commit on the current branch.
* Second parent: the tip of the branch being merged.

Inspect a merge commit:

```bash
git show --summary HEAD
```

Display its parents:

```bash
git rev-list --parents -n 1 HEAD
```

Visualize the history:

```bash
git log --graph --oneline --all
```

An octopus merge can produce a merge commit with more than two parents.

# 🗁 The Git Merge Workflow

A typical feature integration workflow:

### 🖆 Check the Current Branch

```bash
git branch --show-current
```

### 🖆 Check the Working Tree

```bash
git status
```

It is good practice to commit or stash existing work before merging.

### 🖆 Switch to the Target Branch

```bash
git switch main
```

### 🖆 Update the Target Branch

If the branch tracks a remote branch:

```bash
git pull
```

Alternatively, fetch and integrate explicitly:

```bash
git fetch origin
git merge origin/main
```

### 🖆 Merge the Feature Branch

```bash
git merge feature/login
```

### 🖆 Resolve Conflicts if Necessary

If Git reports conflicts:

* Open the affected files.
* Review the conflicting changes.
* Decide what the final content should be.
* Remove conflict markers.
* Stage the resolved files.

```bash
git add <file>
```

### 🖆 Complete the Merge

```bash
git merge --continue
```

If Git indicates that no merge is in progress but a merge commit is still required, use:

```bash
git commit
```

### 🖆 Verify the Result

```bash
git status
git log --graph --oneline --all
```

# 🗁 Using the `git merge` Command

Basic syntax:

```bash
git merge <branch>
```

Merge a branch into the current branch:

```bash
git switch main
git merge feature/login
```

Merge multiple branches:

```bash
git merge feature/login feature/payment
```

Git may perform an octopus merge when multiple branches can be integrated without conflicts. This is not a substitute for carefully integrating dependent or conflicting branches.

Merge a specific commit:

```bash
git merge <commit-hash>
```

Merge without editing the default commit message:

```bash
git merge --no-edit feature/login
```

Preview the merge changes before merging:

```bash
git diff main...feature/login
```

This compares the current `main` tip with the feature branch, using their merge base as the comparison point.

# 🗁 Merge Options

Git provides several options to control merge behavior.

### 🖆 `--no-ff`

Always create a merge commit, even when fast-forwarding is possible.

```bash
git merge --no-ff feature/login
```

Useful when preserving feature integration points is important.

### 🖆 `--ff-only`

Allow only fast-forward merges.

```bash
git merge --ff-only feature/login
```

Git refuses to merge if the histories have diverged.

Useful when maintaining a strictly linear history.

### 🖆 `--squash`

Combine the incoming branch's changes into the current working tree and index without creating a merge commit or recording merge parentage.

```bash
git merge --squash feature/login
git commit -m "Add login feature"
```

The resulting commit is an ordinary commit, not a merge commit.

This creates a simplified history but does not preserve the incoming branch's commit ancestry.

### 🖆 `--no-commit`

Stop before creating the merge commit.

```bash
git merge --no-commit feature/login
```

This allows you to inspect or modify the staged merge result before committing.

**Important:** `--no-commit` alone cannot stop a fast-forward merge. Combine it with `--no-ff` when you need to prevent fast-forwarding:

```bash
git merge --no-ff --no-commit feature/login
```

### 🖆 `--abort`

Cancel an in-progress merge and attempt to restore the pre-merge state.

```bash
git merge --abort
```

### 🖆 `--continue`

Continue an in-progress merge after resolving conflicts and staging the resolved files.

```bash
git add <resolved-file>
git merge --continue
```

### 🖆 `--edit`

Allow editing the merge commit message.

```bash
git merge --edit feature/login
```

### 🖆 `--no-edit`

Accept the automatically generated merge commit message.

```bash
git merge --no-edit feature/login
```

# 🗁 Understanding Merge Conflicts

A merge conflict occurs when Git cannot automatically determine how to combine changes from different branches.

Conflicts can happen when:

* Both branches modify the same lines.
* One branch modifies a file while another deletes it.
* Both branches add a file with the same path.
* Files are renamed differently.
* Changes involve incompatible file or directory structures.
* Binary files are modified independently.

Example:

Suppose `main` contains:

```javascript
const message = "Hello from main";
```

The feature branch contains:

```javascript
const message = "Hello from feature";
```

Both branches modified the same line differently.

When Git attempts to merge them, it may report:

```text
CONFLICT (content): Merge conflict in app.js
Automatic merge failed; fix conflicts and then commit the result.
```

### 🖆 Understanding Conflict Markers

Git inserts conflict markers into the affected file.

```javascript
<<<<<<< HEAD
const message = "Hello from main";
=======
const message = "Hello from feature";
>>>>>>> feature/login
```

Meaning:

| Marker | Meaning |
| --- | --- |
| `<<<<<<< HEAD` | Beginning of the current branch's conflicting content |
| `=======` | Separator between the two versions |
| `>>>>>>> feature/login` | End of the incoming branch's conflicting content |

`HEAD` represents the current branch's version during this merge.

# 🗁 Resolving Merge Conflicts

Conflict resolution means manually deciding what the final content should be.

### 🖆 Identify Conflicted Files

```bash
git status
```

List only unresolved files:

```bash
git diff --name-only --diff-filter=U
```

### 🖆 Open the Conflicted File

Example:

```javascript
<<<<<<< HEAD
const message = "Hello from main";
=======
const message = "Hello from feature";
>>>>>>> feature/login
```

### 🖆 Choose the Correct Result

You can select either version or combine both.

For example:

```javascript
const message = "Hello from main and feature";
```

Remove all conflict markers.

### 🖆 Stage the Resolved File

```bash
git add app.js
```

### 🖆 Check the Merge Status

```bash
git status
```

### 🖆 Complete the Merge

```bash
git merge --continue
```

Git completes the merge and creates the merge commit when required.

# 🗁 Inspecting Conflicts

Git provides commands for examining conflicting changes.

### 🖆 View Unresolved Changes

```bash
git diff
```

### 🖆 View the Current Branch Version

```bash
git diff --ours
```

### 🖆 View the Incoming Branch Version

```bash
git diff --theirs
```

### 🖆 View the Common Ancestor Version

```bash
git diff --base
```

These commands are useful while a merge conflict is in progress.

### 🖆 Inspect Conflict Stages

Git stores three versions of an unresolved file in its index:

| Stage | Meaning |
| --- | --- |
| Stage 1 | Common ancestor version |
| Stage 2 | Current branch version (`ours`) |
| Stage 3 | Incoming branch version (`theirs`) |

Inspect them with:

```bash
git show :1:app.js
git show :2:app.js
git show :3:app.js
```

These stage entries are available for unresolved conflicts. They are not present for files that have already been resolved and staged.

# 🗁 Using a Merge Tool

A merge tool provides a graphical or terminal-based interface for resolving conflicts.

Check available merge tools:

```bash
git mergetool --tool-help
```

Launch the configured merge tool:

```bash
git mergetool
```

Launch it for a specific file:

```bash
git mergetool app.js
```

Configure a preferred tool:

```bash
git config --global merge.tool vimdiff
```

Other commonly used tools include:

* VS Code
* Vimdiff
* Meld
* Beyond Compare
* KDiff3

After resolving conflicts, verify and stage the final files:

```bash
git status
git add <file>
git merge --continue
```

# 🗁 Merge Strategies

Git uses merge strategies to determine how histories and file changes are combined.

### 🖆 `ort`

The modern default strategy for ordinary two-head merges.

```bash
git merge feature/login
```

`ort` handles typical two-branch merges efficiently and supports conflict detection and resolution.

### 🖆 `ours`

The `ours` strategy ignores the other branch's tree entirely while recording a merge.

```bash
git merge -s ours feature/login
```

The resulting tree remains identical to the current branch's tree, but the merge history records the other branch as a parent.

This is useful for specific history-management scenarios, not ordinary feature integration.

### 🖆 `recursive`

The historical default strategy for many two-head merges in older Git versions.

It has largely been replaced by `ort` as the default.

### 🖆 `octopus`

Used for merging multiple branches at once.

```bash
git merge feature/login feature/payment feature/profile
```

It is generally intended for straightforward merges that do not require complex conflict resolution.

### 🖆 `resolve`

A traditional strategy for combining two branches. It is more limited than modern `ort` in handling certain merge situations.

### 🖆 `subtree`

A strategy designed for merging another project's history into a subdirectory of the current project.

### 🖆 Strategy Options vs. Strategies

It is important to distinguish a merge strategy from a strategy option.

**`-s ours`**

Uses the `ours` strategy, which ignores the incoming tree entirely.

```bash
git merge -s ours feature/login
```

**`-X ours`**

Uses the normal merge strategy but prefers the current branch's version when resolving conflicting hunks.

```bash
git merge -X ours feature/login
```

Non-conflicting changes from the incoming branch are still included.

Similarly, `-X theirs` prefers the incoming branch's version for conflicting hunks:

```bash
git merge -X theirs feature/login
```

Neither option replaces the need to review the final result.

# 🗁 Merging Remote Branches

Remote branches are commonly merged after fetching the latest remote information.

### 🖆 Fetch Remote Updates

```bash
git fetch origin
```

This updates remote-tracking references without automatically merging them into your current branch.

### 🖆 View Remote Branches

```bash
git branch -r
```

### 🖆 Merge a Remote-Tracking Branch

```bash
git switch main
git merge origin/feature/login
```

This merges the fetched remote branch into the local `main` branch.

### 🖆 Pull and Merge

You can also use:

```bash
git pull origin feature/login
```

When executed on `main`, this fetches the remote branch and integrates it into the current branch according to the configured pull behavior.

For more control, use separate `fetch` and `merge` commands.

# 🗁 Merging Unrelated Histories

Normally, Git expects the branches being merged to share a common ancestor.

Sometimes two repositories have been initialized independently and have no shared history.

Example:

```text
Repository A: A---B---C

Repository B: X---Y---Z
```

Git cannot find a common ancestor.

Attempting a normal merge may produce:

```text
fatal: refusing to merge unrelated histories
```

To allow the merge:

```bash
git merge other-branch --allow-unrelated-histories
```

This is useful in situations such as combining independently initialized repositories.

Review the resulting changes carefully because the histories have no common ancestor.

# 🗁 Merge vs. Rebase

Both merge and rebase integrate changes from different branches, but they handle history differently.

| Feature | Merge | Rebase |
| --- | --- | --- |
| History | Preserves branch history | Rewrites commit history |
| Merge commit | May create one | Does not create a merge commit |
| Commit hashes | Existing commits remain unchanged | Replayed commits receive new hashes |
| History structure | Can preserve branching | Produces a more linear history |
| Collaboration | Suitable for shared branches | Avoid rewriting shared history |
| Conflict handling | Resolves conflicts during integration | Resolves conflicts while replaying commits |

Example merge:

```bash
git switch main
git merge feature/login
```

Example rebase:

```bash
git switch feature/login
git rebase main
```

Merge preserves the original branch relationships. Rebase moves feature commits onto a new base by replaying them.

The next chapter explores rebasing in detail.

# 🗁 Inspecting Merge History

Git provides several commands for understanding merge history.

### 🖆 Visualize the Commit Graph

```bash
git log --graph --oneline --all
```

### 🖆 Display Merge Commits

```bash
git log --merges --oneline
```

### 🖆 Show the Latest Commit

```bash
git show --summary HEAD
```

### 🖆 Display Commit Parents

```bash
git rev-list --parents -n 1 HEAD
```

### 🖆 Compare Two Branches

Show commits in the feature branch that are not in `main`:

```bash
git log main..feature/login --oneline
```

Compare their changes from the common ancestor:

```bash
git diff main...feature/login
```

### 🖆 Find the Common Ancestor

```bash
git merge-base main feature/login
```

This prints the commit hash of the best common ancestor.

# 🗁 Aborting and Recovering from a Merge

Sometimes a merge becomes complicated or produces unexpected results.

### 🖆 Abort an In-Progress Merge

```bash
git merge --abort
```

Git attempts to return the working tree and index to their pre-merge state.

For the best recovery results, begin merges with a clean working tree. Existing uncommitted changes can make recovery less predictable.

### 🖆 Check the Current State

```bash
git status
```

### 🖆 Inspect Recent History

```bash
git log --oneline --graph -10
```

### 🖆 Recover a Branch Reference

If a merge commit was created and you need to move the current branch back, first inspect the recent reference history:

```bash
git reflog
```

Identify the appropriate previous commit, then reset if necessary:

```bash
git reset --hard <commit-hash>
```

**Important:** `git reset --hard` changes the working tree and index and can discard uncommitted work. Inspect your state and preserve any valuable changes before using it.

For already-published merge commits, `git revert -m` may be more appropriate than rewriting shared history.

# 🗁 Common Problems and Troubleshooting

### 🖆 Already Up to Date

Message:

```text
Already up to date.
```

Meaning:

The current branch already contains all commits from the branch being merged.

Check the history:

```bash
git log --oneline --graph --all
```

### 🖆 Merge Conflict

Message:

```text
Automatic merge failed; fix conflicts and then commit the result.
```

Solution:

```bash
git status
```

Resolve the affected files, then:

```bash
git add <file>
git merge --continue
```

### 🖆 Uncommitted Changes Would Be Overwritten

Message:

```text
Your local changes would be overwritten by merge.
```

Solution:

Commit your changes:

```bash
git add .
git commit -m "Save local changes"
```

Or stash them:

```bash
git stash
git merge feature/login
git stash pop
```

Review any conflicts introduced when applying the stash.

### 🖆 Refusing to Merge Unrelated Histories

Message:

```text
fatal: refusing to merge unrelated histories
```

Solution, when combining independent histories is intentional:

```bash
git merge other-branch --allow-unrelated-histories
```

### 🖆 Cannot Fast-Forward

Message:

```text
fatal: Not possible to fast-forward, aborting.
```

This commonly happens when using `--ff-only` but the branches have diverged.

Review the history:

```bash
git log --graph --oneline --all
```

If a merge commit is appropriate:

```bash
git merge --no-ff feature/login
```

### 🖆 Merge Is Still in Progress

Check:

```bash
git status
```

If conflicts are resolved and staged:

```bash
git merge --continue
```

If you want to cancel:

```bash
git merge --abort
```

# 🖆 Best Practices

* Start a merge with a clean working tree whenever possible.
* Update the target branch before integrating feature work.
* Review incoming changes before merging.
* Use descriptive branch names.
* Resolve conflicts carefully instead of blindly accepting one side.
* Test the application after completing a merge.
* Use `--no-ff` when preserving feature integration points is valuable.
* Use `--ff-only` when enforcing linear history.
* Avoid using `-X ours` or `-X theirs` without understanding their impact.
* Do not confuse `-s ours` with `-X ours`.
* Avoid rewriting shared branch history without team coordination.
* Use `git log --graph --oneline --all` to inspect complex histories.
* Push completed integration commits when appropriate.

# 🖆 Practical Exercises

### 🖆 Exercise: Basic Fast-Forward Merge

Create a repository:

```bash
mkdir merge-practice
cd merge-practice
git init
```

Create an initial commit:

```bash
echo "# Merge Practice" > README.md
git add README.md
git commit -m "Initial commit"
```

Create a feature branch:

```bash
git switch -c feature/login
```

Add a file:

```bash
echo "Login feature" > login.txt
git add login.txt
git commit -m "Add login feature"
```

Return to `main`:

```bash
git switch main
```

Merge the feature:

```bash
git merge feature/login
```

Inspect the history:

```bash
git log --graph --oneline --all
```

### 🖆 Exercise: Three-Way Merge

Create another feature branch:

```bash
git switch -c feature/profile
```

Add a file:

```bash
echo "Profile feature" > profile.txt
git add profile.txt
git commit -m "Add profile feature"
```

Switch to `main`:

```bash
git switch main
```

Make a separate commit:

```bash
echo "Main branch update" > main.txt
git add main.txt
git commit -m "Update main branch"
```

Merge the feature:

```bash
git merge feature/profile
```

Inspect the merge commit:

```bash
git log --graph --oneline --all
```

### 🖆 Exercise: Resolve a Merge Conflict

Create a branch:

```bash
git switch -c feature/conflict
```

Create a file:

```bash
echo "Hello from feature" > message.txt
git add message.txt
git commit -m "Add feature message"
```

Return to `main`:

```bash
git switch main
```

Create the same file with different content:

```bash
echo "Hello from main" > message.txt
git add message.txt
git commit -m "Add main message"
```

Merge the feature branch:

```bash
git merge feature/conflict
```

Git should report a conflict.

Inspect the status:

```bash
git status
```

Open `message.txt`, resolve the conflict, and save the final content.

Complete the merge:

```bash
git add message.txt
git merge --continue
```

Verify:

```bash
git log --graph --oneline --all
```

### 🖆 Exercise: Compare Merge Options

Create a feature branch and add a commit.

Then experiment with:

```bash
git merge --ff-only feature/example
```

```bash
git merge --no-ff feature/example
```

```bash
git merge --squash feature/example
```

Use separate test branches or repositories to compare the resulting histories.

# 🖆 Interview Questions

**What is Git merging?**

Git merging is the process of integrating changes from one branch into another branch.

**What is a fast-forward merge?**

A fast-forward merge moves the current branch reference forward when its tip is an ancestor of the incoming branch tip. It does not create a merge commit.

**What is a three-way merge?**

A three-way merge combines changes using the two branch tips and their best common ancestor.

**What is a merge commit?**

A merge commit records the integration of multiple histories and normally has at least two parents.

**What causes merge conflicts?**

Conflicts occur when Git cannot automatically reconcile changes, such as conflicting edits, file deletions, or competing file additions.

**How do you resolve a merge conflict?**

Edit the conflicted files, remove conflict markers, stage the resolved files, and complete the merge.

**What is the difference between `--no-ff` and `--ff-only`?**

* `--no-ff` forces a merge commit.
* `--ff-only` permits only fast-forward merges.

**What is the difference between `--squash` and a normal merge?**

A normal merge preserves merge ancestry. A squash merge prepares the combined changes for a regular commit without recording merge parentage.

**What is the difference between `-s ours` and `-X ours`?**

* `-s ours` ignores the incoming branch's tree entirely.
* `-X ours` prefers the current branch's version only when resolving conflicting hunks within a normal merge strategy.

**How do you abort a merge?**

```bash
git merge --abort
```

**How do you find the common ancestor of two branches?**

```bash
git merge-base main feature/login
```

**How do you view merge commits?**

```bash
git log --merges --oneline
```

**Can a merge commit have more than two parents?**

Yes. An octopus merge can create a merge commit with more than two parents.

**What is the difference between merge and rebase?**

Merge integrates histories while preserving their original relationships. Rebase replays commits onto a different base, creating new commit hashes.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [Git Documentation — git-merge](https://git-scm.com/docs/git-merge)
* [Git Documentation — git-merge-base](https://git-scm.com/docs/git-merge-base)
* [Git Documentation — git-mergetool](https://git-scm.com/docs/git-mergetool)
* [Git Documentation — git-diff](https://git-scm.com/docs/git-diff)
* [Pro Git Book — Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Pro Git Book — Advanced Merging](https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging)
