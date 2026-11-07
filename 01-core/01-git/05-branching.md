# Git Branching

Branching is one of Git's most powerful features.

A **branch** is a movable reference to a commit that allows developers to work on different versions of a project independently.

Branches make it possible to:

* Develop new features without changing the main development branch.
* Fix bugs independently.
* Experiment with code safely.
* Maintain multiple versions of an application.
* Collaborate with other developers.
* Organize development workflows.

Git branching is lightweight and efficient because creating a branch usually means creating a small reference rather than copying the entire project.

# 🗁 Understanding Branches

A Git branch is a named reference pointing to a commit.

![Git_Branch_Workflow_Infographic](https://res.cloudinary.com/du7tjwbyi/image/upload/v1794031038/Git_Branch_Workflow_gwqbso.png)

For example:

```text
main
  |
  v
A --- B --- C
```

Here, `main` points to commit `C`.

Suppose you create a branch named `development`:

```bash
git branch development
```

The repository now has two branches pointing to the same commit:

```text
main -----------+
                |
                v
A --- B --- C
                ^
                |
development ----+
```

Both branches initially point to the same commit.

When you switch to `development` and create a new commit, only that branch reference moves forward.

```text
A --- B --- C --- D
            ^     ^
            |     |
           main  development
```

The `main` branch remains at `C`, while `development` points to `D`.

### Why Branching Matters

Without branching, developers would need to work directly on a shared line of development.

Branches provide isolation and make parallel development easier.

For example:

```text
main
 ├── feature/user-authentication
 ├── feature/payment-integration
 └── bugfix/login-error
```

Each branch can contain independent changes.

# 🗁 How Git Branches Work

Git branches are lightweight references stored inside the repository.

A local branch typically points to a commit through a reference such as:

```text
refs/heads/main
```

The branch reference moves when new commits are created on that branch.

**Example**

Create a repository:

```bash
mkdir branching-practice
cd branching-practice

git init -b main
```

Create a file and commit it:

```bash
echo "Initial application" > app.txt

git add app.txt
git commit -m "Initial commit"
```

Create another branch:

```bash
git branch development
```

Inspect the branches:

```bash
git branch
```

Output:

```text
  development
* main
```

Both branches point to the same initial commit.

### Important Characteristics

* A branch is a reference, not a separate copy of the repository.
* Creating a branch does not automatically switch to it.
* Each branch can move independently.
* Commits are shared between branches when their histories overlap.
* Branches can later be merged, rebased, or deleted.

# 🗁 Understanding HEAD

`HEAD` is a special reference that identifies the current checkout.

In normal branch-based development, `HEAD` is a symbolic reference to the current local branch.

**Example**:

```text
HEAD -> refs/heads/main
```

This means the current branch is `main`.

### Inspect HEAD

```bash
cat .git/HEAD
```

Possible output:

```text
ref: refs/heads/main
```

You can also inspect it using:

```bash
git symbolic-ref HEAD
```

Show the current branch:

```bash
git branch --show-current
```

Show the current commit:

```bash
git rev-parse HEAD
```

### HEAD and Branches

Suppose the repository has two branches:

```text
A --- B --- C
            ^
            |
           main

development -> C
```

When `HEAD` points to `main`, new commits advance `main`.

When `HEAD` points to `development`, new commits advance `development`.

```text
Before switching:

HEAD -> main

A --- B --- C
            ^
            |
           main
            |
       development
```

After switching:

```text
HEAD -> development

A --- B --- C
            ^
            |
           main
            |
       development
```

The branch references do not move merely because you switch branches. `HEAD` changes its target.

# 🗁 Creating Branches

The `git branch` command creates and manages local branches.

**Create a Branch**

```bash
git branch development
```

Creates a branch named `development` at the current commit.

**Create a Feature Branch**

```bash
git branch feature/user-authentication
```

**Create a Bug-Fix Branch**

```bash
git branch bugfix/login-error
```

**Create a Branch from a Specific Commit**

```bash
git branch feature/new-ui <commit-hash>
```

This creates the branch at the specified commit rather than the current `HEAD`.

**Create a Branch from Another Branch**

```bash
git branch feature/new-ui development
```

This creates `feature/new-ui` at the commit currently referenced by `development`.

**💡 Important**

Creating a branch does not switch the working tree to that branch.

Use `git switch` when you want to move to the newly created branch.

# 🗁 Switching Branches

The `git switch` command changes the current branch.

**Switch to an Existing Branch**

```bash
git switch development
```

Git updates the working tree and index to match the target branch, subject to local-change safety checks.

**Switch Back to Main**

```bash
git switch main
```

**Switch to a Branch with a Slash in Its Name**

```bash
git switch feature/user-authentication
```

**Older Command: git checkout**

Before `git switch` was introduced, developers commonly used:

```bash
git checkout development
```

The `git checkout` command still works, but `git switch` is generally clearer when changing branches.

**Switch to the Previous Branch**

```bash
git switch -
```

This switches to the previously checked-out branch or commit.

**💡 Important**

Git may refuse to switch branches if doing so would overwrite uncommitted changes.

Commit, stash, or otherwise safely handle those changes before switching.

# 🗁 Creating and Switching Branches

Git provides a convenient command to create a branch and switch to it immediately.

**Using git switch**

```bash
git switch -c development
```

Equivalent to:

```bash
git branch development
git switch development
```

**Create a Feature Branch**

```bash
git switch -c feature/user-authentication
```

**Create a Bug-Fix Branch**

```bash
git switch -c bugfix/login-error
```

**Create a Branch from a Specific Starting Point**

```bash
git switch -c feature/new-ui main
```

This creates `feature/new-ui` starting from `main` and switches to it.

**Older Command: `git checkout -b`**

```bash
git checkout -b development
```

This also creates and switches to a new branch.

# 🗁 Listing Branches

Git provides several options for displaying branches.

### List Local Branches

```bash
git branch
```

The current branch is marked with an asterisk.

Example:

```text
  development
  feature/login
* main
```

### List Remote-Tracking Branches

```bash
git branch -r
```

Example:

```text
origin/main
origin/development
origin/feature/login
```

**List All Branches**

```bash
git branch -a
```

This displays both local branches and remote-tracking branches.

### Show the Current Branch

```bash
git branch --show-current
```

### Show Branches with Their Latest Commit

```bash
git branch -v
```

### Show Branches with Upstream Information

```bash
git branch -vv
```

This displays each local branch's upstream relationship, when configured, along with its latest commit.

### Show Merged Branches

```bash
git branch --merged
```

Lists branches whose tips are reachable from the current branch.

### Show Unmerged Branches

```bash
git branch --no-merged
```

Lists branches whose tips are not reachable from the current branch.

# 🗁 Renaming Branches

The `git branch -m` command renames a local branch.

### Rename the Current Branch

```bash
git branch -m development
```

If the current branch is `dev`, this renames it to `development`.

### Rename Another Local Branch

```bash
git branch -m old-name new-name
```

Example:

```bash
git branch -m dev development
```

### Force Rename

```bash
git branch -M old-name new-name
```

The `-M` option allows the destination branch name to be overwritten if it already exists.

Use this carefully.

### Renaming a Branch on a Remote

Renaming a local branch does not automatically rename its counterpart on a remote server.

A typical workflow is:

```bash
git branch -m old-name new-name

git push -u origin new-name

git push origin --delete old-name
```

Before deleting the old remote branch, coordinate with collaborators and update any references to it, such as pull requests and automation settings.

# 🗁 Deleting Branches

Git provides commands for deleting local branches.

### Delete a Local Branch

```bash
git branch -d feature/login
```

The `-d` option refuses to delete a branch if Git determines it contains commits that are not merged into its upstream branch or `HEAD`.

### Force Delete a Local Branch

```bash
git branch -D feature/login
```

The `-D` option forces deletion even if Git considers the branch unmerged.

Use it carefully because the branch reference is removed.

### Delete a Remote Branch

```bash
git push origin --delete feature/login
```

This asks the remote repository to delete the specified branch.

### Alternative Remote Deletion Syntax

```bash
git push origin :feature/login
```

This is an older refspec-based form of remote branch deletion.

### Prune Stale Remote-Tracking Branches

```bash
git fetch --prune
```

Removes local remote-tracking references for branches that no longer exist on the remote.

### 💡IMPORTANT

Deleting a branch does not necessarily delete the commits it referenced.

Commits may remain reachable through other branches, tags, or references. Otherwise, Git may eventually remove unreachable objects during repository maintenance.

# 🗁 Tracking Branches

A tracking branch is a local branch configured to track another branch, commonly a remote-tracking branch.

For example:

```text
Local branch: main
Upstream:     origin/main
```

This relationship allows Git to determine which remote branch should be used for operations such as `git pull` and `git push`.

### Create a Tracking Branch

```bash
git switch --track origin/development
```

This creates a local branch based on `origin/development` and configures its upstream.

### Explicitly Set the Local Branch Name

```bash
git switch -c development --track origin/development
```

### Set Upstream for an Existing Branch

```bash
git branch --set-upstream-to=origin/development development
```

### Set Upstream While Pushing

```bash
git push -u origin development
```

The `-u` option is short for `--set-upstream`.

It establishes the upstream relationship for the local branch.

### Inspect Tracking Information

```bash
git branch -vv
```

Example:

```text
* main        abc1234 [origin/main] Initial commit
  development def5678 [origin/development] Add login feature
```

# 🗁 Upstream Branches

An upstream branch is the branch configured as the tracking counterpart of a local branch.

For example:

```text
Local branch: development
Upstream:     origin/development
```

### Display the Upstream Branch

```bash
git rev-parse --abbrev-ref --symbolic-full-name @{upstream}
```

This returns the configured upstream reference.

### Display Ahead and Behind Information

```bash
git status -sb
```

Example:

```text
## development...origin/development [ahead 2, behind 1]
```

This means:

* The local branch has two commits not present in its upstream.
* The upstream has one commit not present locally.

### Configure the Default Push Behavior

```bash
git config --global push.autoSetupRemote true
```

With this configuration, a first push of a new branch can automatically establish its upstream when applicable.

### 💡IMPORTANT

A remote-tracking branch such as `origin/main` is not the remote branch itself. It is a local reference updated by operations such as `git fetch`.

# 🗁 Branch Configuration

Git stores branch-specific settings in repository configuration.

Inspect branch configuration:

```bash
git config --get-regexp '^branch\.'
```

Example configuration:

```ini
[branch "development"]
    remote = origin
    merge = refs/heads/development
```

This tells Git that the local `development` branch tracks the remote `development` branch on `origin`.

### View the Upstream of a Branch

```bash
git config --get branch.development.remote
git config --get branch.development.merge
```

### Set the Upstream

```bash
git branch --set-upstream-to=origin/development development
```

### Remove the Upstream

```bash
git branch --unset-upstream development
```

# 🗁 Branch Comparison

Git allows developers to compare branches to understand how their histories differ.

### Compare Two Branch Names

```bash
git diff main development
```

Displays file-content differences between the two branch tips.

### Show Commits in One Branch but Not Another

```bash
git log main..development
```

Shows commits reachable from `development` but not from `main`.

### Show Commits in the Opposite Direction

```bash
git log development..main
```

Shows commits reachable from `main` but not from `development`.

### Show Commits Unique to Either Branch

```bash
git log --oneline --left-right main...development
```

The three-dot notation compares the commits reachable from either branch, excluding commits reachable from their merge base.

Output markers:

* `<` indicates commits unique to the left side.
* `>` indicates commits unique to the right side.

### Find the Common Ancestor

```bash
git merge-base main development
```

Returns the best common ancestor commit of the two branches.

### Show Branch Relationships

```bash
git log --oneline --graph --decorate --all
```

This is useful for visualizing branch history.

# 🗁 Detached HEAD

Normally, `HEAD` points to a local branch.

A **detached HEAD** occurs when `HEAD` points directly to a commit instead of a branch reference.

**Example**

```bash
git switch --detach <commit-hash>
```

Or:

```bash
git checkout <commit-hash>
```

Git checks out the specified commit without switching to a local branch.

### What Happens in Detached HEAD?

You can:

* Inspect historical project versions.
* Run tests against an older commit.
* Explore previous implementations.
* Make temporary experimental commits.

However, if you create commits in detached HEAD state, no branch automatically points to them.

### Return to a Branch

```bash
git switch main
```

### Create a Branch from Detached HEAD

If you want to preserve commits created in detached HEAD state:

```bash
git switch -c experiment
```

This creates a branch at the current commit and attaches `HEAD` to it.

### Check Whether HEAD Is Detached

```bash
git status
```

Git displays a message indicating detached HEAD state.

# 🗁 Branch Naming Conventions

Consistent branch naming makes collaboration easier.

Common naming patterns include:

| Prefix | Purpose | Example |
| --- | --- | --- |
| `feature/` | New features | `feature/user-authentication` |
| `bugfix/` | Bug fixes | `bugfix/login-validation` |
| `hotfix/` | Urgent production fixes | `hotfix/payment-failure` |
| `release/` | Release preparation | `release/2.0.0` |
| `docs/` | Documentation changes | `docs/api-guide` |
| `refactor/` | Code restructuring | `refactor/database-layer` |
| `test/` | Test-related work | `test/authentication` |
| `chore/` | Maintenance work | `chore/update-dependencies` |

### Naming Recommendations

* Use lowercase letters.
* Separate words with hyphens.
* Use meaningful descriptions.
* Keep names relatively short.
* Avoid spaces.
* Avoid vague names such as `test`, `temp`, or `new-branch`.
* Follow the team's established naming conventions.

# 🗁 Common Branching Workflows

### Feature Branch Workflow

This workflow creates a separate branch for each feature.

```bash
git switch main
git pull origin main

git switch -c feature/user-authentication

# Make changes
git add .
git commit -m "feat(auth): add user authentication"

git push -u origin feature/user-authentication
```

The feature can then be reviewed and integrated through a pull request.

### Bug-Fix Branch Workflow

```bash
git switch main
git pull origin main

git switch -c bugfix/login-error

# Fix the bug
git add .
git commit -m "fix(auth): resolve login validation error"

git push -u origin bugfix/login-error
```

### Hotfix Branch Workflow

Hotfix branches are commonly created from the production branch or the relevant release branch.

```bash
git switch main
git switch -c hotfix/payment-failure

# Fix the production issue
git add .
git commit -m "fix(payment): handle failed transactions"

git push -u origin hotfix/payment-failure
```

### Release Branch Workflow

A release branch can be used to prepare a version for deployment.

```bash
git switch main
git switch -c release/2.0.0
```

Release-specific changes, testing, and stabilization can happen on this branch.

### Git Flow Overview

Git Flow is a branching model with commonly used branch categories:

* `main`: Production-ready history.
* `develop`: Integration branch for ongoing development.
* `feature/*`: New feature development.
* `release/*`: Release preparation.
* `hotfix/*`: Urgent production fixes.

Git Flow is one possible workflow, not a requirement imposed by Git.

Other approaches, such as GitHub Flow and trunk-based development, use different branching strategies.

# 🖆 Best Practices

* Use branches for isolated features and fixes.
* Keep branches focused on one logical task.
* Create feature branches from an up-to-date base branch.
* Use descriptive branch names.
* Push important branches to a remote repository.
* Delete merged branches when they are no longer needed.
* Avoid force-deleting branches without checking their commits.
* Keep local branches synchronized with their upstream branches.
* Avoid long-lived feature branches when frequent integration is possible.
* Use pull requests or code reviews before integrating important changes.
* Understand detached HEAD before creating commits in that state.
* Coordinate remote branch renaming and deletion with collaborators.

# 🖆 Common Problems and Troubleshooting

### Branch Already Exists

Error:

```text
fatal: a branch named 'development' already exists
```

Inspect branches:

```bash
git branch
```

Switch to the existing branch:

```bash
git switch development
```

### Branch Has Unmerged Changes

Error:

```text
error: The branch 'feature/login' is not fully merged.
```

Git refuses to delete the branch using `-d`.

Inspect its commits:

```bash
git log main..feature/login
```

If the commits are no longer needed:

```bash
git branch -D feature/login
```

Use force deletion only after confirming that the work can be discarded.

### Cannot Switch Branches Because of Local Changes

Error:

```text
error: Your local changes would be overwritten by checkout.
```

Inspect the changes:

```bash
git status
```

Commit them:

```bash
git add .
git commit -m "Save current changes"
```

Or stash them:

```bash
git stash
```

Then switch branches:

```bash
git switch development
```

### Branch Has No Upstream

Git may report:

```text
fatal: no upstream configured for branch
```

Set the upstream:

```bash
git push -u origin development
```

### Remote-Tracking Branch Is Missing

Fetch remote references:

```bash
git fetch origin
```

Inspect:

```bash
git branch -r
```

If the remote branch exists, create a local tracking branch:

```bash
git switch --track origin/development
```

### Local Branch Is Behind the Remote

Fetch and inspect:

```bash
git fetch origin
git status -sb
```

Integrate the upstream changes using the team's chosen merge or rebase workflow.

### Branch Is in Detached HEAD State

Check:

```bash
git status
```

Return to an existing branch:

```bash
git switch main
```

Or preserve current work on a new branch:

```bash
git switch -c saved-work
```

# 🖆 Practical Exercises

### Exercise: Create and Switch Branches

Create a repository:

```bash
mkdir git-branching
cd git-branching

git init -b main

echo "Main application" > app.txt
git add app.txt
git commit -m "Initial commit"
```

Create a development branch:

```bash
git branch development
```

Switch to it:

```bash
git switch development
```

Create another commit:

```bash
echo "Development feature" >> app.txt

git add app.txt
git commit -m "Add development feature"
```

Inspect the history:

```bash
git log --oneline --graph --all
```

### Exercise: Create Feature Branches

```bash
git switch main

git switch -c feature/login

echo "Login feature" > login.txt
git add login.txt
git commit -m "feat: add login feature"

git switch main
git switch -c feature/register

echo "Registration feature" > register.txt
git add register.txt
git commit -m "feat: add registration feature"
```

Inspect all branches:

```bash
git branch -a
git log --oneline --graph --all
```

### Exercise: Rename and Delete Branches

Create a branch:

```bash
git switch main
git switch -c old-feature
```

Rename it:

```bash
git branch -m new-feature
```

Return to main:

```bash
git switch main
```

Delete it:

```bash
git branch -d new-feature
```

### Exercise: Track a Remote Branch

Create a local branch and push it:

```bash
git switch -c feature/profile
git push -u origin feature/profile
```

Inspect tracking information:

```bash
git branch -vv
```

### Exercise: Explore Detached HEAD

Find a commit:

```bash
git log --oneline
```

Check out an older commit:

```bash
git switch --detach <commit-hash>
```

Inspect:

```bash
git status
git branch --show-current
```

Create a branch to preserve any new commits:

```bash
git switch -c experiment
```

# 🖆 Interview Questions

### Beginner

**What is a Git branch?**

A branch is a movable reference to a commit that provides an independent line of development.

**Which command creates a branch?**

```bash
git branch <branch-name>
```

**Which command switches branches?**

```bash
git switch <branch-name>
```

**How do you create and switch to a branch in one command?**

```bash
git switch -c <branch-name>
```

**How do you list all branches?**

```bash
git branch -a
```

### Intermediate

**What is the difference between git branch and git switch?**

`git branch` manages branch references, while `git switch` changes the current checkout.

**What is HEAD in Git?**

`HEAD` identifies the current checkout. Normally, it symbolically points to the current local branch.

**What is a tracking branch?**

A local branch configured to track another branch, commonly a remote-tracking branch.

**What is the difference between git branch -d and git branch -D?**

`-d` performs a safety check before deletion. `-D` forces deletion.

**How do you delete a remote branch?**

```bash
git push origin --delete <branch-name>
```

### Advanced

**Why are Git branches lightweight?**

A branch is essentially a reference to a commit rather than a separate copy of the entire repository.

**What is detached HEAD?**

A state in which `HEAD` points directly to a commit instead of a branch reference.

**What is an upstream branch?**

The branch configured as the tracking counterpart of a local branch.

**What is the difference between `git log main..development` and `git log main...development`?**

`main..development` shows commits reachable from `development` but not from `main`. `main...development` with `git log` shows commits unique to either side of their merge base.

**Does creating a branch copy all project files?**

No. Creating a branch normally creates a lightweight reference to an existing commit.

**What happens when you delete a branch?**

Git removes the branch reference. Commits may remain accessible through other references or may eventually become unreachable.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official Git Documentation**

* [Git Documentation](https://git-scm.com/docs)
* [git-branch](https://git-scm.com/docs/git-branch)
* [git-switch](https://git-scm.com/docs/git-switch)
* [git-checkout](https://git-scm.com/docs/git-checkout)
* [git-push](https://git-scm.com/docs/git-push)
* [git-fetch](https://git-scm.com/docs/git-fetch)
* [git-merge-base](https://git-scm.com/docs/git-merge-base)
* [git-symbolic-ref](https://git-scm.com/docs/git-symbolic-ref)

**Additional Learning**

* [Pro Git — Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
* [Pro Git — Branch Management](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management)
* [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow)
