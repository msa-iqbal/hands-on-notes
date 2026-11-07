# Git Staging and Commits

Git staging and commits are fundamental parts of the version-control workflow.

Before Git records changes in its history, developers select which modifications should be included in the next commit.

This process involves two important concepts:

* **Staging:** Preparing selected changes for a commit.
* **Committing:** Recording the staged snapshot in repository history.

Git provides flexibility to stage individual files, multiple files, specific lines, or selected portions of a file.

This allows developers to create clean, focused, and meaningful commits.

# 🗁 Understanding the Git Workflow

Git uses three primary areas when managing changes:

* Working tree
* Staging area (index)
* Repository history

**Git Workflow Diagram**

```text
        Working Tree
        (Edit files)
             |
             | git add
             |
             v
        Staging Area
        (Prepare changes)
             |
             | git commit
             |
             v
       Repository History
       (Record snapshot)
```

### Working Tree

The working tree contains the project files you currently edit.

Changes made here are not automatically included in the next commit.

### Staging Area

The staging area stores the content selected for the next commit.

It allows you to choose exactly which changes should be recorded.

### Repository History

When you create a commit, Git records the staged snapshot as a new commit object.

The commit becomes part of the repository's history.

### Important Workflow Commands

```bash
git status
git add <file>
git diff
git diff --staged
git commit -m "Commit message"
```

# 🗁 Understanding the Staging Area

The staging area is also called the **index**.

It is an intermediate area between the working tree and repository history.

When you stage a file, Git records the content and metadata that should be included in the next commit.

The staging area is not simply a list of filenames. It maintains information about the versions of files prepared for committing.

**Example**

Suppose your project contains:

```text
my-project/
├── README.md
├── app.js
└── style.css
```

You modify all three files.

However, you only want to commit `README.md` and `app.js`.

Stage the selected files:

```bash
git add README.md app.js
```

Now:

* `README.md` is staged.
* `app.js` is staged.
* `style.css` remains unstaged.

Create the commit:

```bash
git commit -m "Update project documentation and application logic"
```

Only the staged versions are included in that commit.

### Why the Staging Area Matters

* Allows selective commits.
* Helps separate unrelated changes.
* Makes code reviews easier.
* Prevents accidental inclusion of unfinished work.
* Allows developers to review changes before committing.

# 🗁 Tracked and Untracked Files

Git classifies files based on whether they are already tracked by the repository.

### Untracked Files

An untracked file exists in the working tree but is not currently tracked by Git.

Example:

```bash
touch notes.txt
git status
```

Git may display:

```text
Untracked files:
  notes.txt
```

Start tracking it by staging:

```bash
git add notes.txt
```

### Tracked Files

A tracked file is known to Git.

Tracked files can be:

* Unmodified
* Modified
* Staged for the next commit

### File Status Overview

| File State | Meaning |
| --- | --- |
| Untracked | Git is not tracking the file |
| Unmodified | File matches the version in the current commit |
| Modified | Tracked file has unstaged changes |
| Staged | Changes are prepared for the next commit |

### Important Distinction

A file becomes tracked when it is added to Git's index.

After committing it, Git records its content in repository history.

# 🗁 Staging Changes

The `git add` command stages changes for the next commit.

### Stage a Specific File

```bash
git add README.md
```

### Stage Multiple Files

```bash
git add README.md app.js style.css
```

### Stage All Changes in the Current Directory

```bash
git add .
```

This stages changes under the current directory, including new, modified, and deleted files.

### Stage All Changes Across the Working Tree

```bash
git add -A
```

This stages additions, modifications, and deletions throughout the working tree.

### Stage Modified and Deleted Tracked Files

```bash
git add -u
```

This updates the index for tracked files without staging new untracked files.

### Stage Files Using a Pattern

```bash
git add "*.js"
```

This stages matching JavaScript files according to the pathspec.

### Interactive Staging

```bash
git add -i
```

Opens an interactive interface for selecting changes to stage.

### Stage Changes Patch by Patch

```bash
git add -p
```

This allows you to select individual change hunks instead of staging an entire file.

It is especially useful when a file contains unrelated modifications.

# 🗁 Inspecting Staged and Unstaged Changes

Git provides several commands for reviewing changes before committing.

### Check Repository Status

```bash
git status
```

Displays staged, unstaged, and untracked changes.

### Show Unstaged Changes

```bash
git diff
```

Compares the working tree against the staging area.

It shows modifications that have not yet been staged.

### Show Staged Changes

```bash
git diff --staged
```

Displays the changes currently prepared for the next commit.

The following command is equivalent:

```bash
git diff --cached
```

### Show Changes Between Working Tree and Last Commit

```bash
git diff HEAD
```

Displays differences between the current working tree and the current commit, including staged and unstaged tracked-file changes.

### Show a Summary of Changes

```bash
git diff --stat
```

Displays a summary of modified files and line changes.

### Show Changed Filenames

```bash
git diff --name-only
```

Lists filenames with differences.

### Show Staged Filenames

```bash
git diff --staged --name-only
```

Lists files with staged changes.

**Example Workflow**

```bash
# Modify a file
echo "New feature" >> README.md

# Inspect unstaged changes
git diff

# Stage the file
git add README.md

# Inspect staged changes
git diff --staged

# Check repository status
git status
```

# 🗁 Unstaging Changes

Sometimes you stage files accidentally or decide that certain changes should not be included in the next commit.

Git provides commands to remove changes from the staging area without necessarily discarding them from the working tree.

### Unstage a Specific File

```bash
git restore --staged README.md
```

The file becomes unstaged, but its working-tree modifications remain.

### Unstage All Staged Changes

```bash
git restore --staged .
```

Removes staged changes from the index while preserving the working-tree files.

### Alternative: Use git reset

```bash
git reset HEAD README.md
```

This unstages the file while preserving its working-tree modifications.

### Important Distinction

| Command | Effect |
| --- | --- |
| `git restore --staged <file>` | Unstage a file |
| `git reset HEAD <file>` | Unstage a file |
| `git restore <file>` | Discard unstaged changes to a tracked file |
| `git reset --hard` | Reset index and working tree to a commit |

Be careful to distinguish unstaging changes from discarding them.

# 🗁 Understanding Git Commits

A commit is a recorded snapshot of the repository's staged content.

Each commit contains:

* A reference to a tree object representing the project snapshot.
* Parent commit reference or references, except for root commits.
* Author information.
* Committer information.
* Timestamps.
* Commit message.

Git commits form a history through parent-child relationships.

### Commit Structure

```text
Commit
├── Tree
│   ├── Blob
│   ├── Blob
│   └── Tree
├── Parent Commit
├── Author
├── Committer
└── Commit Message
```

### Commit History

A typical commit history looks like:

```text
A --- B --- C --- D
```

Each commit points to its parent.

* `A` is the initial commit.
* `B` is based on `A`.
* `C` is based on `B`.
* `D` is based on `C`.

This structure allows Git to reconstruct project history.

### Important Properties

* Commits are identified by object IDs.
* Commits are immutable objects.
* A commit records a snapshot, not merely a list of file changes.
* A commit can have multiple parents, as with merge commits.
* Changing a commit's content or metadata creates a different commit object.

# 🗁 Creating Commits

The `git commit` command records staged changes in repository history.

### Create a Commit

```bash
git add README.md
git commit -m "Add project documentation"
```

### Commit All Modified Tracked Files

```bash
git commit -a -m "Update application files"
```

The `-a` option automatically stages modifications and deletions to tracked files before committing.

It does not automatically stage new untracked files.

### Open the Commit Message Editor

```bash
git commit
```

Git opens the configured editor so you can write a commit message.

### Create an Empty Commit

```bash
git commit --allow-empty -m "Trigger deployment"
```

This creates a commit without staged file changes.

It can be useful for triggering automation or recording an event.

### Sign a Commit

```bash
git commit -S -m "Add signed commit"
```

This creates a cryptographically signed commit using the configured signing key.

### Commit with a Specific Author

```bash
git -c user.name="Developer Name" \
    -c user.email="developer@example.com" \
    commit -m "Add feature"
```

This overrides the author and committer configuration for that command.

For more complex author and committer control, Git also provides options such as `--author`.

# 🗁 Commit Messages

A commit message describes the purpose of a change.

Good commit messages make repository history easier to understand.

### Recommended Structure

```text
Short summary of the change

Optional detailed explanation of:
- What changed
- Why it changed
- Important implementation details
```

### Good Commit Messages

```text
Add user authentication
Fix navigation menu alignment
Update API error handling
Refactor database connection logic
Add pagination to product listing
```

### Poor Commit Messages

```text
update
changes
fix
test
new code
final
```

These messages do not communicate the purpose of the changes.

### Conventional Commit Format

A widely used commit-message convention is Conventional Commits.

Format:

```text
<type>[optional scope]: <description>
```

Common types:

| Type | Purpose |
| --- | --- |
| `feat` | Introduce a new feature |
| `fix` | Fix a bug |
| `docs` | Documentation changes |
| `style` | Formatting changes |
| `refactor` | Code restructuring |
| `test` | Add or update tests |
| `perf` | Performance improvements |
| `build` | Build system or dependency changes |
| `ci` | CI/CD configuration changes |
| `chore` | Maintenance tasks |

Examples:

```bash
git commit -m "feat(auth): add password reset"
git commit -m "fix(api): handle invalid request payload"
git commit -m "docs: update installation guide"
git commit -m "refactor(user): simplify profile validation"
```

### Breaking Changes

Conventional Commits can communicate breaking changes using `!`:

```text
feat(api)!: remove deprecated endpoints
```

A detailed description can also include a `BREAKING CHANGE:` footer.

# 🗁 Inspecting Commit History

Git provides several commands for viewing repository history.

### Show Commit History

```bash
git log
```

Displays detailed commit history.

### Compact Commit History

```bash
git log --oneline
```

Displays one line per commit.

### Show Graphical Commit History

```bash
git log --oneline --graph --all
```

Displays a compact graph of commits across available references.

### Show the Latest Commit

```bash
git log -1
```

### Show the Latest Commit in One Line

```bash
git log -1 --oneline
```

### Show Commit Statistics

```bash
git log --stat
```

Displays changed files and line statistics for commits.

### Show Commit Details

```bash
git show HEAD
```

Displays the latest commit's metadata and changes.

### Show a Specific Commit

```bash
git show <commit-hash>
```

### Show Commit History for a File

```bash
git log -- README.md
```

### Show File History with Changes

```bash
git log -p -- README.md
```

### Limit the Number of Commits

```bash
git log -5
```

Displays the latest five commits.

### Filter by Author

```bash
git log --author="Developer Name"
```

### Filter by Commit Message

```bash
git log --grep="authentication"
```

### Filter by Date

```bash
git log --since="2026-01-01" --until="2026-12-31"
```

# 🗁 Commit Metadata

Every commit contains metadata describing who created the change and who recorded it.

### Author

The author is the person who originally wrote the changes.

### Committer

The committer is the person who created the commit object.

In ordinary commits, the author and committer are usually the same person.

They can differ when commits are applied, cherry-picked, rebased, or otherwise recreated by another person.

### Inspect Commit Metadata

```bash
git show --format=fuller HEAD
```

### Show Author Information

```bash
git log -1 --format="%an <%ae>"
```

### Show Committer Information

```bash
git log -1 --format="%cn <%ce>"
```

### Show Commit Hash

```bash
git rev-parse HEAD
```

### Show Commit Date

```bash
git log -1 --format="%cd"
```

### Common Format Placeholders

| Placeholder | Meaning |
| --- | --- |
| `%H` | Full commit hash |
| `%h` | Abbreviated commit hash |
| `%an` | Author name |
| `%ae` | Author email |
| `%cn` | Committer name |
| `%ce` | Committer email |
| `%s` | Commit subject |
| `%b` | Commit body |
| `%cd` | Commit date |

Example:

```bash
git log -1 --format="%h - %an - %s"
```

# 🗁 Amending Commits

The `git commit --amend` command replaces the latest commit with a new commit.

It is commonly used to correct a commit message or include forgotten changes.

### Change the Latest Commit Message

```bash
git commit --amend -m "Correct commit message"
```

### Add Forgotten Changes to the Latest Commit

```bash
git add forgotten-file.js
git commit --amend --no-edit
```

The `--no-edit` option keeps the existing commit message.

### Change the Author

```bash
git commit --amend --author="Developer Name <developer@example.com>"
```

### Important

Amending a commit creates a replacement commit with a different object ID.

Avoid amending commits that have already been shared with others unless you understand the consequences of rewriting shared history.

# 🗁 Comparing Commits

Git provides several commands to compare changes between commits and repository states.

### Compare Working Tree with Staging Area

```bash
git diff
```

### Compare Staging Area with Latest Commit

```bash
git diff --staged
```

### Compare Two Commits

```bash
git diff <commit-1> <commit-2>
```

Example:

```bash
git diff HEAD~1 HEAD
```

Compares the previous commit with the current commit.

### Compare Two Branches

```bash
git diff main development
```

Shows the differences between the tips of the two branches.

### Compare a Specific File Between Commits

```bash
git diff HEAD~1 HEAD -- README.md
```

### Compare Commit Statistics

```bash
git diff --stat HEAD~1 HEAD
```

### Understand HEAD~ and HEAD^

For a commit with a single parent:

* `HEAD~1` refers to its first parent.
* `HEAD~2` refers to the first parent's first parent.
* `HEAD^` refers to its first parent.

For merge commits, parent selection matters. `HEAD^2` refers to the second parent.

# 🗁 Removing Files

Git provides different commands for deleting files from the working tree and repository index.

### Remove a File from Git and Working Tree

```bash
git rm README.md
```

This stages the deletion and removes the file from the working tree.

Commit the deletion:

```bash
git commit -m "Remove outdated documentation"
```

### Remove a File from Git Tracking but Keep It Locally

```bash
git rm --cached config.local.json
```

This stages the removal from version control but leaves the file in the working tree.

This is useful when a file was accidentally tracked and should no longer be included in future commits.

Add it to `.gitignore` if it should remain untracked.

### Remove a Directory

```bash
git rm -r old-directory/
```

### Important

Removing a file from tracking does not remove it from previous commits.

If a secret was committed, removing the file in a later commit does not erase the secret from repository history.

# 🗁 Ignoring Files

The `.gitignore` file tells Git which untracked files and paths should be ignored.

It is commonly used for:

* Dependencies.
* Build outputs.
* Log files.
* Temporary files.
* Environment-specific configuration.
* Editor-generated files.

### Create a `.gitignore` File

```bash
touch .gitignore
```

Example:

```gitignore
# Dependencies
node_modules/

# Environment files
.env
.env.*

# Build output
dist/
build/

# Logs
*.log

# Temporary files
*.tmp

# Operating system files
.DS_Store
Thumbs.db
```

### Important Rules

* `.gitignore` does not affect files already tracked by Git.
* Patterns can match files and directories.
* A leading slash anchors a pattern relative to the location of the `.gitignore` file.
* A trailing slash specifies a directory pattern.
* `!` can negate an ignore pattern, with some parent-directory limitations.

### Stop Tracking an Already Tracked File

```bash
git rm --cached .env
```

Then add it to `.gitignore`:

```gitignore
.env
```

Commit the change:

```bash
git add .gitignore
git commit -m "Stop tracking environment configuration"
```

### Inspect Ignore Rules

```bash
git check-ignore -v .env
```

This shows which ignore rule matches the specified path.

# 🗁 Advanced Staging Techniques

### Stage Individual Changes

```bash
git add -p
```

This opens an interactive patch selection interface.

Common responses include:

| Option | Meaning |
| --- | --- |
| `y` | Stage this hunk |
| `n` | Do not stage this hunk |
| `q` | Quit |
| `a` | Stage this and remaining hunks in the file |
| `d` | Do not stage this or remaining hunks in the file |
| `s` | Split the current hunk into smaller hunks, if possible |
| `e` | Manually edit the patch |

This is useful when a single file contains multiple unrelated changes.

### Stage a Specific Patch

```bash
git add -p README.md
```

### Stage an Entire File

```bash
git add README.md
```

### Stage All Changes

```bash
git add -A
```

### Stage Only Tracked Changes

```bash
git add -u
```

### Commit Only Selected Files

```bash
git commit README.md -m "Update documentation"
```

This form commits the specified tracked files using their current working-tree content, rather than relying on the ordinary staged snapshot for those paths. It does not stage unrelated changes.

For most workflows, explicitly staging changes and then using `git commit` is easier to reason about.

# 🖆 Best Practices

* Review changes before staging.
* Prefer focused commits that represent one logical change.
* Write meaningful commit messages.
* Avoid committing generated files unless the project requires them.
* Never commit credentials or secrets.
* Use `.gitignore` for local and generated files.
* Use `git add -p` when a file contains unrelated modifications.
* Review staged changes with `git diff --staged`.
* Avoid unnecessary empty commits.
* Avoid amending commits that have already been shared.
* Keep commits small enough to review and understand.
* Run relevant tests before creating important commits.

# 🖆 Common Problems and Troubleshooting

### Nothing to Commit

Message:

```text
nothing to commit, working tree clean
```

Cause:

There are no staged or unstaged changes to commit.

Solution:

```bash
git status
```

Make changes or stage existing changes before committing.

### Changes Not Included in Commit

Cause:

The changes were not staged, or the staged version did not contain the latest modifications.

Solution:

```bash
git status
git add <file>
git diff --staged
git commit -m "Update files"
```

### Accidentally Staged a File

Solution:

```bash
git restore --staged <file>
```

This preserves the file's working-tree changes.

### Accidentally Staged a Secret

If the secret has not been committed:

```bash
git restore --staged .env
```

Then add it to `.gitignore`.

If the secret has already been committed or pushed, removing it from the latest snapshot is not sufficient. Rotate the exposed credential and follow an appropriate history-cleanup process.

### Commit Message Is Incorrect

If the commit is the latest commit and has not been shared:

```bash
git commit --amend -m "Correct message"
```

### New Files Are Not Included by `git commit -a`

Cause:

`git commit -a` only stages modifications and deletions of tracked files.

Solution:

```bash
git add new-file.js
git commit -m "Add new file"
```

### File Is Still Being Tracked Despite `.gitignore`

Cause:

The file was already tracked before the ignore rule was added.

Solution:

```bash
git rm --cached <file>
```

Then commit the change.

# 🖆 Practical Exercises

### Exercise: Create Your First Commit

Create a repository:

```bash
mkdir commit-practice
cd commit-practice

git init -b main
```

Create a file:

```bash
echo "# Commit Practice" > README.md
```

Stage and commit:

```bash
git add README.md
git commit -m "docs: add project README"
```

Inspect:

```bash
git status
git log --oneline
```

### Exercise: Stage Selected Files

Create multiple files:

```bash
echo "Application" > app.js
echo "Styles" > style.css
echo "Documentation" > notes.md
```

Stage only two files:

```bash
git add app.js style.css
```

Inspect:

```bash
git status
git diff --staged
```

Commit:

```bash
git commit -m "feat: add application and styles"
```

Observe that `notes.md` remains untracked.

### Exercise: Practice Unstaging

Modify a tracked file:

```bash
echo "New content" >> README.md
git add README.md
```

Unstage it:

```bash
git restore --staged README.md
```

Verify:

```bash
git status
```

The modification should remain in the working tree.

### Exercise: Practice Partial Staging

Add two separate changes to the same file.

```bash
echo "Feature A" >> README.md
echo "Feature B" >> README.md
```

Run:

```bash
git add -p README.md
```

Stage only one hunk if Git presents the changes separately.

Inspect:

```bash
git diff
git diff --staged
```

### Exercise: Amend a Commit

Create a commit:

```bash
echo "Initial content" > example.txt
git add example.txt
git commit -m "Add example"
```

Add another change:

```bash
echo "Additional content" >> example.txt
git add example.txt
```

Amend the latest commit:

```bash
git commit --amend --no-edit
```

Inspect the result:

```bash
git log --oneline -3
```

### Exercise: Work with .gitignore

Create a file:

```bash
echo ".env" >> .gitignore
echo "SECRET=value" > .env
```

Stage the ignore file:

```bash
git add .gitignore
git commit -m "chore: ignore environment files"
```

Check the ignored file:

```bash
git status
git check-ignore -v .env
```

# 🖆 Interview Questions

### Beginner

**What is the Git staging area?**

The staging area is an intermediate area where selected changes are prepared before being recorded in a commit.

**What does git add do?**

It adds new files or updates file content in the staging area.

**What does git commit do?**

It records the staged snapshot as a new commit in repository history.

**What is the difference between staged and unstaged changes?**

Staged changes are prepared for the next commit. Unstaged changes exist in the working tree but have not been added to the index.

**What is the purpose of git status?**

It displays the current branch and the state of tracked, untracked, staged, and unstaged changes.

### Intermediate

**What is the difference between git add . and git add -A?**

`git add .` stages changes under the current directory. `git add -A` stages additions, modifications, and deletions throughout the working tree.

**What is the difference between git diff and git diff --staged?**

`git diff` compares the working tree with the index. `git diff --staged` compares the index with the current commit.

**What does git commit -a do?**

It automatically stages modifications and deletions of tracked files before committing. It does not include new untracked files.

**What is git commit --amend used for?**

It replaces the latest commit, commonly to correct its message or include additional changes.

**What is the difference between author and committer?**

The author originally wrote the changes. The committer created the commit object.

### Advanced

**What is Git's index?**

The index is Git's staging-area data structure. It records the content and metadata prepared for the next commit.

**Can you commit only part of a file?**

Yes. Use `git add -p` to interactively stage selected change hunks.

**Does .gitignore affect already tracked files?**

No. Already tracked files remain tracked until removed from the index.

**Are Git commits mutable?**

Commit objects are immutable. Replacing a commit creates a new commit object with a different object ID.

**Why should you avoid amending published commits?**

Amending changes commit identity and rewrites history. This can create synchronization problems for collaborators who already have the original commit.

**What is the difference between git restore --staged and git restore?**

`git restore --staged` unstages changes. `git restore` can discard unstaged working-tree changes.
