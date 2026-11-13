# Advanced Git

Git provides many advanced features beyond everyday commands such as `add`, `commit`, `push`, `pull`, and `merge`.

Understanding these capabilities helps developers:

* Recover accidentally deleted commits.
* Inspect Git's internal data structures.
* Automate repository workflows.
* Debug historical changes.
* Manage multiple working directories.
* Optimize large repositories.
* Maintain clean and reliable commit history.
* Diagnose complex merge and rebase problems.

This chapter covers advanced Git concepts and techniques useful in professional software development.

# 🗁 Understanding Git Internals

Git is a content-addressable distributed version control system.

Instead of storing only differences between file versions, Git stores objects identified by cryptographic hashes.

A simplified repository structure:

```text
project/
│
├── .git/
│   ├── HEAD
│   ├── config
│   ├── objects/
│   ├── refs/
│   ├── logs/
│   ├── index
│   └── hooks/
│
└── src/
    └── app.js
```

Important internal components:

| Component | Purpose |
| --- | --- |
| `objects/` | Stores Git objects |
| `refs/` | Stores branch and tag references |
| `HEAD` | Identifies the current checkout |
| `index` | Stores the staging area |
| `config` | Repository configuration |
| `logs/` | Stores reference update history |
| `hooks/` | Contains hook scripts |

Inspect the Git directory:

```bash
git rev-parse --git-dir
```

Display the repository's top-level directory:

```bash
git rev-parse --show-toplevel
```

# 🗁 Git Object Model

Git stores four fundamental object types:

* Blob
* Tree
* Commit
* Tag

### 🖆 Blob Object

A blob stores file content.

It does not store the filename or directory structure.

Create a blob manually:

```bash
echo "Hello Git" | git hash-object -w --stdin
```

The `-w` option writes the object into the repository's object database.

### 🖆 Tree Object

A tree object represents a directory structure.

It contains references to:

* Blobs representing files.
* Other trees representing subdirectories.

Inspect a commit's tree:

```bash
git ls-tree HEAD
```

### 🖆 Commit Object

A commit object contains:

* Root tree reference.
* Parent commit reference or references.
* Author information.
* Committer information.
* Commit message.

Inspect a commit object:

```bash
git cat-file -p HEAD
```

### 🖆 Annotated Tag Object

An annotated tag is a Git object containing metadata such as:

* Target object.
* Tag name.
* Tagger information.
* Tag message.
* Optional cryptographic signature.

Inspect an annotated tag:

```bash
git cat-file -p v1.0.0
```

### 🖆 Inspect Git Objects

Display an object's type:

```bash
git cat-file -t HEAD
```

Display an object's content:

```bash
git cat-file -p HEAD
```

Display the object database:

```bash
git count-objects -v
```

### 🖆 Object Relationships

```text
Commit
  |
  v
Tree
  |
  +------ Blob (file-1)
  |
  +------ Blob (file-2)
  |
  +------ Tree (directory)
              |
              +------ Blob (file-3)
```

A commit points to a tree representing the project snapshot. Trees reference other trees and blobs.

# 🗁 Git References

Git references are human-readable names that point to Git objects.

Common references include:

* Local branches.
* Remote-tracking branches.
* Tags.

Examples:

```text
refs/heads/main
refs/heads/development
refs/remotes/origin/main
refs/tags/v1.0.0
```

### 🖆 List References

```bash
git show-ref
```

### 🖆 List Local Branch References

```bash
git for-each-ref refs/heads/
```

### 🖆 List Remote-Tracking References

```bash
git for-each-ref refs/remotes/
```

### 🖆 Inspect a Reference

```bash
git rev-parse main
```

### 🖆 Symbolic References

Some references point to other references rather than directly to objects.

For example, `HEAD` usually points symbolically to the current local branch:

```text
ref: refs/heads/main
```

Inspect it:

```bash
cat .git/HEAD
```

For repositories using a separate Git directory, use:

```bash
git rev-parse --git-path HEAD
```

# 🗁 HEAD and Detached HEAD

`HEAD` identifies the current checkout.

Normally, it points to a local branch.

Example:

```text
HEAD
 |
 v
refs/heads/main
 |
 v
Commit C
```

### 🖆 Inspect HEAD

```bash
git log -1 --oneline
```

### 🖆 Detached HEAD

A detached HEAD occurs when you check out a commit directly instead of checking out a branch.

```bash
git checkout <commit-hash>
```

Or:

```bash
git switch --detach <commit-hash>
```

In this state, `HEAD` points directly to a commit rather than a branch reference.

### 🖆 Work in Detached HEAD

You can inspect files, run tests, and even create commits.

However, commits created in detached HEAD are not automatically associated with a branch.

To preserve your work, create a branch:

```bash
git switch -c experimental-work
```

This creates a branch at the current commit and attaches HEAD to it.

# 🗁 Git Reflog

The reflog records updates to references such as `HEAD` and local branches.

It is especially useful for recovering commits after operations such as:

* Reset.
* Rebase.
* Amend.
* Branch deletion.
* Accidental checkout changes.

### 🖆 View HEAD Reflog

```bash
git reflog
```

Example:

```text
a1b2c3d HEAD@{0}: reset: moving to HEAD~1
e4f5g6h HEAD@{1}: commit: Add authentication
```

### 🖆 View a Branch Reflog

```bash
git reflog show main
```

### 🖆 View Reflog with Dates

```bash
git reflog --date=local
```

### 🖆 Recover a Previous Commit

Suppose you accidentally reset your branch:

```bash
git reset --hard HEAD~1
```

Find the previous commit:

```bash
git reflog
```

Recover it by creating a new branch:

```bash
git switch -c recovered-work <commit-hash>
```

Or move the current branch back:

```bash
git reset --hard <commit-hash>
```

The branch-reset approach replaces the current branch position, so inspect your working tree and commit history before using it.

### 🖆 Reflog Expiration

Reflog entries are not permanent.

Git periodically expires old entries, and unreachable objects may eventually be removed during garbage collection.

Useful commands:

```bash
git reflog expire --all --dry-run
```

```bash
git reflog expire --expire=90.days.ago --all
```

Avoid manually expiring reflogs unless you understand the recovery implications.

# 🗁 Recovering Lost Commits

Git sometimes retains objects that are no longer reachable from a branch or tag.

These are called unreachable or dangling objects.

### 🖆 Find Unreachable Objects

```bash
git fsck --full
```

### 🖆 Find Dangling Commits

```bash
git fsck --full --no-reflogs --unreachable
```

### 🖆 Inspect a Recovered Commit

```bash
git show <commit-hash>
```

### 🖆 Create a Recovery Branch

```bash
git branch recovered-commit <commit-hash>
```

This makes the commit reachable through a branch reference.

**Important:** Do not run aggressive garbage collection before attempting recovery. Unreachable objects may be permanently removed once they expire and are pruned.

# 🗁 Git Hooks

Git hooks are scripts that execute automatically when specific Git events occur.

They help automate repository workflows and enforce development rules.

Hooks are stored in:

```text
.git/hooks/
```

### 🖆 Common Git Hooks

| Hook | Execution Time |
| --- | --- |
| `pre-commit` | Before creating a commit |
| `prepare-commit-msg` | Before opening the commit message editor |
| `commit-msg` | After the commit message is prepared |
| `post-commit` | After a commit is created |
| `pre-push` | Before pushing commits |
| `post-checkout` | After checkout or switch |
| `post-merge` | After a successful merge |
| `pre-rebase` | Before a rebase starts |
| `pre-receive` | Before accepting pushed references on the receiving server |
| `update` | Before updating an individual reference on the receiving server |
| `post-receive` | After references are updated on the receiving server |

### 🖆 Create a Pre-Commit Hook

Create:

```text
.git/hooks/pre-commit
```

Example:

```bash
#!/bin/sh

echo "Running pre-commit checks..."

npm run lint

if [ $? -ne 0 ]; then
    echo "Lint failed. Commit aborted."
    exit 1
fi

echo "Pre-commit checks passed."
```

Make it executable:

```bash
chmod +x .git/hooks/pre-commit
```

Now Git runs the script before creating a commit.

### 🖆 Create a Commit Message Hook

Create:

```text
.git/hooks/commit-msg
```

Example:

```bash
#!/bin/sh

commit_msg=$(cat "$1")

if ! echo "$commit_msg" | grep -Eq '^(feat|fix|docs|refactor|test|chore): .+'; then
    echo "Invalid commit message format."
    echo "Example: feat: add login feature"
    exit 1
fi
```

This enforces a simple Conventional Commits-style format.

### 🖆 Skip Hooks

```bash
git commit --no-verify -m "Emergency fix"
```

This skips certain verification hooks, including `pre-commit` and `commit-msg`.

Use it only when necessary.

### 🖆 Share Hooks with a Team

By default, hooks inside `.git/hooks/` are not version-controlled.

You can maintain shared hooks in a project directory:

```text
project/
├── .githooks/
│   ├── pre-commit
│   └── commit-msg
└── .git/
```

Configure Git to use them:

```bash
git config core.hooksPath .githooks
```

Commit the `.githooks` directory so the team can use the same hook scripts.

Each developer must configure the hooks path unless the setup is automated.

# 🗁 Git Aliases

Git aliases allow you to create shortcuts for frequently used commands.

### 🖆 Create an Alias

```bash
git config --global alias.st status
```

Now:

```bash
git st
```

### 🖆 Common Aliases

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.sw switch
git config --global alias.df diff
```

### 🖆 Log Aliases

```bash
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Usage:

```bash
git lg
```

### 🖆 Last Commit Alias

```bash
git config --global alias.last "log -1 HEAD"
```

### 🖆 Unstage Alias

```bash
git config --global alias.unstage "restore --staged"
```

Usage:

```bash
git unstage file.txt
```

### 🖆 Inspect Aliases

```bash
git config --get-regexp '^alias\.'
```

### 🖆 Remove an Alias

```bash
git config --global --unset alias.st
```

# 🗁 Advanced Git Configuration

Git configuration controls repository behavior, command defaults, identity, aliases, and formatting.

### 🖆 View All Configuration

```bash
git config --list
```

### 🖆 Show Configuration with Its Source

```bash
git config --list --show-origin
```

### 🖆 Configure Default Branch Name

```bash
git config --global init.defaultBranch main
```

### 🖆 Configure Pull Strategy

Use rebase:

```bash
git config --global pull.rebase true
```

Use merge:

```bash
git config --global pull.rebase false
```

Allow only fast-forward pulls:

```bash
git config --global pull.ff only
```

### 🖆 Configure Automatic Pruning

```bash
git config --global fetch.prune true
```

This automatically removes stale remote-tracking references during fetch operations.

### 🖆 Configure Color Output

```bash
git config --global color.ui auto
```

### 🖆 Configure Commit Template

Create a template:

```text
# Summary:

# Details:

# Testing:
```

Save it as:

```text
~/.gitmessage
```

Configure Git:

```bash
git config --global commit.template ~/.gitmessage
```

Git uses this template when preparing commit messages in supported editor workflows.

### 🖆 Configuration Scopes

| Scope | Command | Location |
| --- | --- | --- |
| System | `git config --system` | System configuration |
| Global | `git config --global` | User configuration |
| Local | `git config --local` | Repository configuration |
| Worktree | `git config --worktree` | Worktree-specific configuration |

Local settings generally override global and system settings when the same key is configured at multiple levels.

# 🗁 Git Worktrees

Git worktrees allow you to check out multiple branches from the same repository into separate working directories.

This is useful when you need to:

* Work on multiple branches simultaneously.
* Review another developer's branch.
* Apply urgent fixes without interrupting current work.
* Run tests against different branches.
* Avoid repeatedly switching branches.

### 🖆 Create a Worktree

```bash
git worktree add ../project-feature feature
```

This creates a new working directory for the existing `feature` branch.

### 🖆 Create a New Branch and Worktree

```bash
git worktree add -b hotfix ../project-hotfix main
```

This creates a new branch named `hotfix` from `main` and checks it out in `../project-hotfix`.

### 🖆 List Worktrees

```bash
git worktree list
```

### 🖆 Remove a Worktree

```bash
git worktree remove ../project-feature
```

### 🖆 Prune Stale Worktree Metadata

```bash
git worktree prune
```

### 🖆 Example Structure

```text
workspace/
│
├── project/
│   └── main branch
│
├── project-feature/
│   └── feature branch
│
└── project-hotfix/
    └── hotfix branch
```

All worktrees share the same underlying repository object database.

A branch normally cannot be checked out in multiple worktrees simultaneously.

# 🗁 Interactive Rebase

Interactive rebase gives you control over how commits are rewritten and organized.

It is useful for:

* Squashing commits.
* Reordering commits.
* Editing commit messages.
* Removing unnecessary commits.
* Splitting commits.
* Cleaning up feature branch history.

### 🖆 Start Interactive Rebase

```bash
git rebase -i HEAD~3
```

Git opens an editor containing the selected commits.

Example:

```text
pick a1b2c3d Add login page
pick e4f5g6h Fix login validation
pick i7j8k9l Update login styles
```

### 🖆 Squash Commits

Change the instructions:

```text
pick a1b2c3d Add login page
squash e4f5g6h Fix login validation
squash i7j8k9l Update login styles
```

Git combines the selected commits into one commit and allows you to edit the resulting message.

### 🖆 Reword a Commit

```text
reword a1b2c3d Add login page
pick e4f5g6h Fix login validation
pick i7j8k9l Update login styles
```

### 🖆 Drop a Commit

```text
pick a1b2c3d Add login page
drop e4f5g6h Temporary debugging
pick i7j8k9l Update login styles
```

### 🖆 Edit a Commit

```text
edit a1b2c3d Add login page
pick e4f5g6h Fix login validation
pick i7j8k9l Update login styles
```

Git pauses at that commit so you can modify it.

### 🖆 Continue or Abort

Continue:

```bash
git rebase --continue
```

Abort:

```bash
git rebase --abort
```

Skip the current commit:

```bash
git rebase --skip
```

Avoid rewriting commits that other developers are already using unless the team has coordinated the change.

# 🗁 Advanced Revision Selection

Git provides powerful revision syntax for selecting commits and comparing history.

### 🖆 HEAD References

```bash
HEAD
HEAD~1
HEAD~2
HEAD~3
```

`HEAD~2` means the second first-parent ancestor of HEAD.

### 🖆 Parent References

For a merge commit:

```bash
HEAD^1
HEAD^2
```

These identify the first and second parents.

### 🖆 Select a Commit Range

```bash
git log main..feature
```

Shows commits reachable from `feature` but not from `main`.

### 🖆 Symmetric Difference

```bash
git log main...feature
```

Shows commits reachable from either branch but not both.

### 🖆 Commit Before a Date

```bash
git log --before="2026-01-01"
```

### 🖆 Commit After a Date

```bash
git log --after="2026-01-01"
```

### 🖆 Search Commit Messages

```bash
git log --grep="authentication"
```

### 🖆 Search by Author

```bash
git log --author="Developer Name"
```

### 🖆 Search Changes to a Specific File

```bash
git log --follow -- src/app.js
```

`--follow` attempts to continue history across file renames. It is intended for a single file path.

# 🗁 Git Bisect

`git bisect` helps identify the commit that introduced a bug by performing a binary search through commit history.

Instead of manually inspecting every commit, Git repeatedly checks out commits between a known good and known bad state.

### 🖆 Start Bisect

```bash
git bisect start
```

Mark the current commit as bad:

```bash
git bisect bad
```

Mark a known working commit as good:

```bash
git bisect good <commit-hash>
```

Git checks out an intermediate commit.

Test the application and mark the result.

If the commit is good:

```bash
git bisect good
```

If the commit is bad:

```bash
git bisect bad
```

Continue until Git identifies the first bad commit.

### 🖆 Finish Bisect

```bash
git bisect reset
```

This returns the working tree to the original branch and commit.

### 🖆 Automated Bisect

If you have a test script that returns:

* Exit code `0` when the commit is good.
* A nonzero exit code when the commit is bad.

You can automate the search:

```bash
git bisect start
git bisect bad
git bisect good <known-good-commit>

git bisect run npm test

git bisect reset
```

Ensure the test is reliable and can run against historical versions of the project.

# 🗁 Git Blame

`git blame` identifies the commit and author responsible for each line of a file.

### 🖆 Basic Usage

```bash
git blame src/app.js
```

### 🖆 Inspect a Specific Range

```bash
git blame -L 10,30 src/app.js
```

This displays line history for lines 10 through 30.

### 🖆 Ignore Whitespace Changes

```bash
git blame -w src/app.js
```

### 🖆 Show Commit Details

```bash
git blame -p src/app.js
```

### 🖆 Inspect the Commit

After identifying a commit:

```bash
git show <commit-hash>
```

`git blame` is useful for understanding code history, but authorship information does not necessarily identify who originally designed or approved the logic.

# 🗁 Git Grep

`git grep` searches tracked files in a repository.

### 🖆 Search for Text

```bash
git grep "TODO"
```

### 🖆 Search Case-Insensitively

```bash
git grep -i "authentication"
```

### 🖆 Search a Specific Directory

```bash
git grep "useState" -- src/
```

### 🖆 Search with Line Numbers

```bash
git grep -n "function login"
```

### 🖆 Search a Specific Commit

```bash
git grep "database" HEAD~3
```

### 🖆 Search All Tracked Files

```bash
git grep "API_KEY"
```

Avoid treating a simple search result as proof that a secret is safe or unsafe; review the surrounding code and repository history.

# 🗁 Git Archive

`git archive` creates an archive of a repository tree without including the `.git` directory.

### 🖆 Create a ZIP Archive

```bash
git archive --format=zip --output=project.zip HEAD
```

### 🖆 Create a Tar Archive

```bash
git archive --format=tar --output=project.tar HEAD
```

### 🖆 Archive a Specific Branch

```bash
git archive --format=zip --output=release.zip main
```

### 🖆 Archive a Specific Directory

```bash
git archive --format=zip --output=src.zip HEAD src/
```

### 🖆 Add a Root Directory Prefix

```bash
git archive \
  --format=zip \
  --prefix=project/ \
  --output=project.zip \
  HEAD
```

Archives contain the selected tree contents, not the repository's complete Git history.

# 🗁 Git Bundle

Git bundles package Git objects and references into a single file.

They are useful for:

* Transferring repositories without a live remote.
* Offline collaboration.
* Creating portable repository backups.
* Sharing branches between isolated environments.

### 🖆 Create a Bundle

```bash
git bundle create project.bundle --all
```

### 🖆 Verify a Bundle

```bash
git bundle verify project.bundle
```

### 🖆 Inspect Available References

```bash
git bundle list-heads project.bundle
```

### 🖆 Clone from a Bundle

```bash
git clone project.bundle project-copy
```

### 🖆 Fetch from a Bundle

```bash
git fetch project.bundle main
```

A bundle can include only selected references and the objects required by them. Check the bundle's advertised references before relying on it for a particular branch.

# 🗁 Git Maintenance and Garbage Collection

Git stores objects in its object database.

Over time, repositories can accumulate loose objects and other data that benefit from maintenance.

### 🖆 Run Garbage Collection

```bash
git gc
```

This performs repository maintenance, including object packing and pruning according to Git's maintenance rules.

### 🖆 Aggressive Garbage Collection

```bash
git gc --aggressive
```

This performs more expensive optimization and can take significantly longer.

It is not necessary for routine maintenance in most repositories.

### 🖆 Prune Unreachable Objects

```bash
git prune
```

This removes unreachable objects that meet Git's pruning criteria.

Be careful: unreachable objects can include commits you may still want to recover.

### 🖆 Inspect Object Storage

```bash
git count-objects -v
```

### 🖆 Run Maintenance

```bash
git maintenance run
```

### 🖆 Register Scheduled Maintenance

```bash
git maintenance start
```

### 🖆 Stop Scheduled Maintenance

```bash
git maintenance stop
```

Git maintenance can automate tasks such as loose-object cleanup, commit-graph updates, and incremental repacking.

# 🗁 Git Performance Optimization

Large repositories can become slow because of:

* Many loose objects.
* Large histories.
* Large binary files.
* Expensive history traversal.
* Large working trees.
* Repeated network transfers.

### 🖆 Use Shallow Clones

```bash
git clone --depth 1 <repository-url>
```

This downloads limited history.

### 🖆 Use Partial Clones

```bash
git clone --filter=blob:none <repository-url>
```

This requests a partial clone that initially omits file-content blobs and retrieves them when needed.

The remote server must support the required filtering behavior.

### 🖆 Use Sparse Checkout

Sparse checkout allows you to populate only selected parts of the working tree.

Initialize:

```bash
git sparse-checkout init --cone
```

Select directories:

```bash
git sparse-checkout set src docs
```

Inspect configuration:

```bash
git sparse-checkout list
```

Disable sparse checkout:

```bash
git sparse-checkout disable
```

### 🖆 Use Commit Graphs

Git can maintain commit-graph data to improve certain history operations.

Write a commit graph:

```bash
git commit-graph write --reachable
```

### 🖆 Avoid Large Binary Files in Normal Git History

For large binary assets, consider Git LFS.

Git LFS stores large file content outside the normal Git object database and tracks pointer files in the repository.

Official documentation:

<https://git-lfs.com/>

# 🗁 Advanced Diff and Log Techniques

### 🖆 Show a Graphical Commit History

```bash
git log --oneline --graph --decorate --all
```

### 🖆 Show Changed Files

```bash
git diff --name-only
```

### 🖆 Show Change Statistics

```bash
git diff --stat
```

### 🖆 Compare Two Commits

```bash
git diff <commit-a> <commit-b>
```

### 🖆 Compare Branches

```bash
git diff main..feature
```

For `git diff`, the two-dot form compares the trees at the two specified endpoints.

### 🖆 Compare a Commit with Its Parent

```bash
git show --stat HEAD
```

### 🖆 Show Word-Level Changes

```bash
git diff --word-diff
```

### 🖆 Show Changes in a Specific File

```bash
git diff main feature -- src/app.js
```

### 🖆 Search History for String Changes

```bash
git log -S "oldFunctionName" --all
```

`-S` finds commits where the number of occurrences of the specified string changes.

For a regular-expression-based search of added and removed lines:

```bash
git log -G "function\s+login" --all
```

# 🗁 Advanced Merge Techniques

### 🖆 Preview Merge Changes

```bash
git diff HEAD...feature
```

For a three-dot diff, Git compares the merge base of the two commits against the second commit.

### 🖆 Abort a Merge

```bash
git merge --abort
```

### 🖆 Continue After Resolving Conflicts

```bash
git add .
git merge --continue
```

### 🖆 Merge Without Automatically Committing

```bash
git merge --no-commit --no-ff feature
```

This allows you to inspect the result before creating the merge commit.

### 🖆 Inspect Merge Parents

```bash
git show --no-patch --pretty=raw HEAD
```

A merge commit normally has two or more parent references.

### 🖆 Find the Merge Base

```bash
git merge-base main feature
```

The merge base is the best common ancestor Git identifies for a three-way merge.

# 🗁 Git Security

Git repositories can expose sensitive information if secrets are committed.

Common examples include:

* API keys.
* Database passwords.
* Private tokens.
* SSH private keys.
* Cloud credentials.
* Environment files containing secrets.

### 🖆 Prevent Accidental Secret Commits

Add sensitive files to `.gitignore`:

```gitignore
.env
.env.*
!.env.example

*.pem
*.key
secrets/
```

Remember that `.gitignore` does not remove files that are already tracked.

### 🖆 Inspect Tracked Files

```bash
git ls-files
```

### 🖆 Check for Sensitive Content

```bash
git grep -n "API_KEY"
```

### 🖆 Remove a File from Tracking

```bash
git rm --cached .env
```

Then commit the change:

```bash
git commit -m "Stop tracking environment file"
```

This removes the file from the current version but does not erase it from previous commits.

### 🖆 Handle Exposed Secrets

If a secret has been committed or pushed:

* Revoke or rotate the secret immediately.
* Determine whether it was exposed publicly or shared with others.
* Remove the secret from repository history when necessary.
* Coordinate rewritten history with collaborators.
* Add appropriate secret scanning to prevent recurrence.

Deleting a secret in a new commit does not remove it from earlier history.

# 🗁 Advanced Git Workflows

### 🖆 Clean Feature Branch Workflow

```bash
git switch main
git pull --ff-only

git switch -c feature/authentication

# Make changes

git add .
git commit -m "feat: add authentication"

git fetch origin
git rebase origin/main

git push -u origin feature/authentication
```

### 🖆 Maintain a Clean Commit History

```bash
git rebase -i HEAD~4
```

Use interactive rebase to organize commits before sharing them.

### 🖆 Recover from an Accidental Reset

```bash
git reflog
```

Identify the previous commit:

```bash
git switch -c recovery <commit-hash>
```

### 🖆 Fix a Bug with Bisect

```bash
git bisect start
git bisect bad
git bisect good <known-good-commit>

git bisect run npm test

git bisect reset
```

### 🖆 Work on an Urgent Fix Without Switching Branches

```bash
git worktree add -b hotfix ../project-hotfix main
```

Work independently in the new directory.

### 🖆 Automate Commit Validation

Create a shared hook:

```text
.githooks/pre-commit
```

Configure it:

```bash
git config core.hooksPath .githooks
```

Use it to run linting, formatting, or tests before commits.

# 🖆 Troubleshooting

### 🖆 Cannot Recover a Lost Commit

Inspect the reflog:

```bash
git reflog --all
```

Search unreachable objects:

```bash
git fsck --full --unreachable
```

Inspect possible commits:

```bash
git show <commit-hash>
```

Create a branch if the desired commit is found:

```bash
git branch recovery <commit-hash>
```

### 🖆 Hook Does Not Execute

Check whether the hook exists:

```bash
ls -l .git/hooks/
```

Check executable permissions:

```bash
chmod +x .git/hooks/pre-commit
```

Check the configured hooks directory:

```bash
git config --get core.hooksPath
```

Ensure the hook has a valid interpreter declaration, such as:

```bash
#!/bin/sh
```

### 🖆 Worktree Branch Is Already Checked Out

Git normally prevents checking out the same branch in multiple worktrees.

Inspect existing worktrees:

```bash
git worktree list
```

Use a different branch or remove the existing worktree if it is no longer needed.

### 🖆 Rebase Has Conflicts

Inspect conflicts:

```bash
git status
```

Resolve them and stage the files:

```bash
git add .
```

Continue:

```bash
git rebase --continue
```

Abort if necessary:

```bash
git rebase --abort
```

### 🖆 Repository Is Too Large

Inspect object storage:

```bash
git count-objects -v
```

Check large tracked files:

```bash
git ls-files
```

Consider:

* Removing unnecessary large files from future commits.
* Using Git LFS for large assets.
* Using partial clones or sparse checkout.
* Reviewing repository history for oversized objects.
* Running appropriate maintenance.

### 🖆 Secret Was Accidentally Committed

Stop using the exposed credential and rotate or revoke it.

Remove it from the working tree and add the appropriate ignore rule.

If necessary, rewrite repository history with a suitable history-cleaning tool, then coordinate the rewritten history with collaborators.

# 🖆 Best Practices

* Understand Git's object model and reference structure.
* Use reflog before assuming commits are permanently lost.
* Create recovery branches before performing destructive operations.
* Use interactive rebase to clean up private, unpublished history.
* Avoid rewriting shared history without coordination.
* Use hooks to automate quality checks.
* Store shared hooks in a version-controlled directory.
* Use worktrees for independent parallel development.
* Use `git bisect` to locate regressions efficiently.
* Use `git blame` to investigate code history, not to assign blame to developers.
* Keep secrets outside Git repositories.
* Use Git LFS for large binary files.
* Perform repository maintenance when appropriate.
* Prefer partial clones and sparse checkout for large repositories.
* Document advanced workflows for the development team.

# 🖆 Practical Exercises

### 🖆 Exercise: Explore Git Internals

* Create a new repository.
* Add and commit a file.
* Inspect the commit using `git cat-file -p HEAD`.
* Inspect the tree using `git ls-tree HEAD`.
* Display object storage using `git count-objects -v`.
* Inspect references using `git show-ref`.

### 🖆 Exercise: Recover a Commit

* Create multiple commits.
* Run `git reset --hard HEAD~1`.
* Inspect the reflog.
* Identify the missing commit.
* Recover it using a new branch.

### 🖆 Exercise: Create Git Hooks

* Create a `.githooks` directory.
* Add a `pre-commit` hook.
* Make it executable.
* Configure `core.hooksPath`.
* Test the hook by attempting a commit.

### 🖆 Exercise: Create Git Aliases

Create aliases for:

* `status`
* `branch`
* `checkout`
* `log`
* `diff`

Inspect your configuration and test each alias.

### 🖆 Exercise: Use Git Worktrees

* Create a repository with a `main` branch.
* Create a feature branch.
* Add a separate worktree for the feature branch.
* Modify files in both worktrees.
* Inspect the worktree list.
* Remove the additional worktree.

### 🖆 Exercise: Use Git Bisect

* Create a sequence of commits.
* Introduce a bug in one commit.
* Identify a known good commit.
* Start bisect.
* Find the first bad commit.
* Reset bisect.

### 🖆 Exercise: Create a Git Bundle

* Create a repository with several commits.
* Create a bundle containing all references.
* Verify the bundle.
* Clone another repository from it.
* Inspect its branches and history.

# 🖆 Interview Questions

**What are the four main Git object types?**

Blob, tree, commit, and annotated tag.

**What is a Git blob?**

An object that stores file content without storing its filename or directory structure.

**What is a Git tree object?**

An object representing a directory structure through references to blobs and other trees.

**What is a gitlink?**

A special index and tree entry used to record the commit selected for a submodule.

**What is the purpose of HEAD?**

HEAD identifies the current checkout, normally through a symbolic reference to the current branch.

**What is a detached HEAD state?**

A state where HEAD points directly to a commit instead of a branch reference.

**What is Git reflog?**

A local record of reference updates that helps inspect and recover previous branch and HEAD positions.

**How can you recover a commit after `git reset --hard`?**

Inspect `git reflog`, identify the previous commit, and create a branch or reset the current branch to that commit.

**What are Git hooks?**

Scripts that Git executes automatically when specific repository events occur.

**What is the difference between client-side and server-side hooks?**

Client-side hooks run in the local repository during operations such as committing or pushing. Server-side hooks run on the receiving server during operations such as accepting pushed references.

**What are Git aliases?**

Custom shortcuts for Git commands, configured through Git configuration.

**What is Git worktree?**

A feature that allows multiple working directories to share the same underlying repository.

**What is Git bisect?**

A binary-search-based tool for identifying the commit that introduced a bug.

**What is Git blame?**

A tool that shows the commit and author associated with each line of a file.

**What is Git garbage collection?**

A maintenance process that optimizes repository storage and cleans up eligible unreachable objects.

**What is a partial clone?**

A clone that downloads only selected object types initially and retrieves omitted objects when needed.

**What is sparse checkout?**

A feature that limits the working tree to selected directories or files.

**What is Git bundle?**

A portable file containing Git objects and references that can be used to transfer repository data without a live remote.

**What is the difference between `git archive` and `git bundle`?**

`git archive` packages selected repository files. `git bundle` packages Git objects and references, preserving Git history for the included references.

**How can you inspect unreachable Git objects?**

```bash
git fsck --full --unreachable
```

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

* [Official Git Documentation](https://git-scm.com/docs)
* [Git Internals](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain)
* [Git Reflog](https://git-scm.com/docs/git-reflog)
* [Git Hooks](https://git-scm.com/docs/githooks)
* [Git Worktree](https://git-scm.com/docs/git-worktree)
* [Git Bisect](https://git-scm.com/docs/git-bisect)
* [Git Maintenance](https://git-scm.com/docs/git-maintenance)
* [Git Configuration](https://git-scm.com/docs/git-config)
* [Git Bundle](https://git-scm.com/docs/git-bundle)
* [Git Archive](https://git-scm.com/docs/git-archive)
* [Git LFS](https://git-lfs.com/)
* [Pro Git Book](https://git-scm.com/book/en/v2)
