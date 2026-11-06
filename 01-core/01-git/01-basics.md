# Git Basics — Version Control Fundamentals

Software development is an iterative process.

Developers continuously modify source code, introduce new features, fix bugs, refactor implementations, and update project configurations.

Every modification introduces a potential problem:

* What changed?
* When did it change?
* Who changed it?
* Why was it changed?
* What did the project look like before the modification?
* How can we recover an earlier version?
* How can multiple developers work on the same project?

These problems become increasingly difficult as projects grow.

Version Control Systems (VCS) solve these problems by maintaining a structured history of changes.

Git is one of the most widely used version control systems in modern software engineering.

### Version Control

Version control is a system for recording, managing, and tracking changes made to files over time.

It allows developers to maintain multiple versions of a project and inspect how the project evolves.

For example:

```text
my-project/
├── src/
├── package.json
└── README.md
```

Suppose you develop a web application.

Initially, your application has a basic login system.

Later, you introduce:

1. User registration.
2. Password validation.
3. Authentication middleware.
4. Password reset functionality.
5. OAuth authentication.

Without version control, tracking these modifications manually becomes difficult.

With Git, each meaningful change can be recorded as a commit.

```text
Initial project
      |
      v
Add login functionality
      |
      v
Add registration
      |
      v
Add authentication middleware
      |
      v
Add password reset
```

You can inspect the history and understand how the application evolved.

# 🗁 What Is Version Control?

A Version Control System is software that records changes to files and allows users to manage different versions of those files.

A VCS generally provides:

* Change tracking.
* Version history.
* Comparison between versions.
* Restoration of previous versions.
* Collaboration support.
* Conflict detection and resolution.
* Branching and merging.

### Version Control Example

Imagine a configuration file:

```javascript
const config = {
    port: 3000,
    environment: "development"
};
```

You later change it:

```javascript
const config = {
    port: 8080,
    environment: "production"
};
```

A version control system can record both versions and help you inspect the difference.

```diff
const config = {
-   port: 3000,
-   environment: "development"
+   port: 8080,
+   environment: "production"
};
```

This makes it possible to understand exactly what changed.

### What Does Version Control Track?

Depending on the system and configuration, version control can track:

| Item | Description |
| --- | --- |
| File creation | Newly added files |
| File modification | Changes to existing files |
| File deletion | Removed files |
| File renaming | Changes in file paths |
| Directory structure | Changes represented through tracked files and trees |
| Commit history | Recorded project snapshots |
| Author information | Who created a commit |
| Timestamps | When changes were recorded |
| Branch history | Different development timelines |

Git primarily tracks file contents and their relationships within the repository. It does not independently version empty directories.

# 🗁 Why Version Control Is Important

Version control is important because it makes software development more predictable, traceable, and collaborative.

### Change Tracking

Developers can inspect modifications made to a project.

```bash
git diff
```

This displays unstaged changes between the working directory and the index.

### History Management

Git maintains a history of commits.

```bash
git log
```

You can inspect previous changes and identify when particular functionality was introduced.

### Rollback and Recovery

If a change introduces a problem, Git provides different mechanisms to recover or reverse it.

Examples:

```bash
git restore
git revert
git reset
```

These commands have different purposes and consequences, which will be covered in dedicated chapters.

### Parallel Development

Multiple developers can work on different features using separate branches.

```text
                 main
                   |
          -------------------
          |                 |
     feature/login     feature/payment
          |                 |
      Development        Development
          |                 |
          ---------+--------
                   |
                 Merge
                   |
                  main
```

### Collaboration

Developers can share changes through remote repositories.

Git supports collaboration workflows using:

* Clone
* Fetch
* Pull
* Push
* Merge
* Rebase

### Accountability

Commit metadata helps identify:

* The author.
* The committer.
* The commit message.
* The commit timestamp.
* The parent commit or commits.

This makes project history easier to audit and understand.

### Project Stability

Version control allows teams to maintain stable production code while developing new features independently.

# 🗁 Types of Version Control Systems

Version control systems are commonly divided into three major categories:

1. Local Version Control Systems.
2. Centralized Version Control Systems.
3. Distributed Version Control Systems.

### Local Version Control System (LVCS)

A Local Version Control System maintains version history on a single computer.

Developers can save different versions of files locally.

**Architecture**

```text
Developer's Computer
        |
        v
+------------------+
| Project Files    |
+------------------+
        |
        v
+------------------+
| Local Version DB |
+------------------+
```

**Characteristics**

* Version history exists locally.
* No central server is required.
* Simple to use.
* Limited collaboration capabilities.
* History may be lost if the local system fails and no backup exists.

 **Examples**

* Manual file backups.
* RCS (Revision Control System).

**Limitations**

Local version control is insufficient for teams that need coordinated development and shared project history.

### Centralized Version Control System (CVCS)

A Centralized Version Control System stores the main repository on a central server.

Developers connect to that server to obtain files and submit changes.

**Architecture**

```text
              Central Server
             +---------------+
             | Repository    |
             | Version Data  |
             +---------------+
                    |
         +----------+----------+
         |          |          |
         v          v          v
      Client A   Client B   Client C
```

**Characteristics**

* A central repository exists.
* Developers communicate with the central server.
* Access permissions can be centrally managed.
* Collaboration is organized around one repository.
* Many operations require network connectivity.

**Examples**

* SVN (Apache Subversion).
* CVS (Concurrent Versions System).
* Perforce Helix Core (centralized deployment model).

**Advantages**

* Centralized repository management.
* Easier centralized access control.
* Straightforward collaboration model.

**Disadvantages**

* Server availability becomes important.
* Network connectivity is needed for many repository operations.
* The central server can become a bottleneck or single point of failure for repository access.

### Distributed Version Control System (DVCS)

A Distributed Version Control System gives each developer a complete repository copy, including project history.

Developers can perform most version control operations locally.

**Architecture**

```text
                 Remote Repository
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
       Client A      Client B      Client C
          |             |             |
      Local Repo    Local Repo    Local Repo
      Full History  Full History  Full History
```

**Characteristics**

* Each clone contains repository history.
* Most operations happen locally.
* Developers can work offline.
* Multiple repositories can exchange changes.
* Branching and merging are fundamental workflows.

**Examples**

* Git.
* Mercurial.

**Advantages**

* Offline development.
* Fast local operations.
* Distributed collaboration.
* Multiple repository copies.
* Flexible branching and merging.

**Disadvantages**

* Collaboration workflows require coordination.
* Understanding distributed history can take time.
* Repository copies may consume additional storage.

### Comparison of Version Control Systems

| Feature | Local VCS | Centralized VCS | Distributed VCS |
| --- | --- | --- | --- |
| Repository location | Local | Central server | Multiple locations |
| Full history on each client | Usually local only | Usually server-side | Yes |
| Offline commits | Yes | Generally no | Yes |
| Central server required | No | Yes | No |
| Collaboration | Limited | Centralized | Distributed |
| Examples | RCS | SVN, CVS | Git, Mercurial |

### 💡 Important Concept

Git is a Distributed Version Control System.

However, GitHub and similar platforms commonly provide hosted remote repositories that teams use as shared collaboration points.

# 🗁 What Is Git?

Git is a free and open-source distributed version control system.

It was created by Linus Torvalds in 2005.

Git was initially developed to support the Linux kernel development process.

### Important Characteristics

* **Distributed**: Each clone contains the repository's history and objects.
* **Snapshot-Based**: Git records project snapshots through commits.
* **Content-Addressable**: Git identifies objects using cryptographic hashes.
* **Branching**: Branches are lightweight references that point to commits.
* **Integrity**: Git uses hashes to identify objects and detect content changes.
* **Performance**: Most operations are performed locally.

### Git Is Not GitHub

Git and GitHub serve different purposes.

| Git                           | GitHub                                        |
| ----------------------------- | --------------------------------------------- |
| Version control software      | Hosting and collaboration platform            |
| Manages repository history    | Hosts Git repositories                        |
| Runs locally                  | Primarily cloud-based                         |
| Provides branches and commits | Provides pull requests and code reviews       |
| Open-source                   | Proprietary platform with free and paid plans |
|                               |                                               |

Git can work without GitHub.

You can use Git locally, on a private server, or with other hosting platforms.

# 🗁 Git Design Philosophy

Git was designed around several important principles.

Understanding these principles helps explain why Git behaves differently from many other version control systems.

### Everything Is Local First

Git performs most operations locally.

For example:

```bash
git status
git log
git diff
git branch
git commit
```

These commands generally do not require network access.

Remote operations are needed when exchanging changes with another repository.

### Data Integrity

Git uses cryptographic hashes to identify repository objects.

An object's identity is based on its content and object type.

This makes accidental or unauthorized content changes detectable.

### Branching Is Lightweight

Creating a Git branch generally creates a new reference pointing to a commit.

It does not duplicate the entire project directory or history.

```bash
git branch feature-login
```

### Immutable Object Storage

Git objects are treated as immutable.

When content changes, Git creates new objects rather than modifying existing objects in place.

This supports reliable historical snapshots.

### Efficient Storage

Git uses content-addressable objects, compression, and packfiles to store repository data efficiently.

Identical content can be reused rather than stored repeatedly as separate objects.

### History Through Commits

Commits form a directed acyclic graph (DAG).

Each commit references a tree and one or more parent commits.

This structure allows Git to represent different development histories.

# 🗁 How Git Stores Data

One of Git's most important design decisions is its approach to storing project information.

Git primarily stores snapshots of project states.

It does not simply maintain a traditional list of line-by-line differences as its fundamental storage model.

### Snapshot-Based Storage

Imagine a project containing three files:

```text
project/
├── index.html
├── style.css
└── app.js
```

At the first commit, Git records a snapshot representing the tracked project state.

```text
Commit A
├── index.html
├── style.css
└── app.js
```

Now suppose only `app.js` changes.

The next commit represents the new project state:

```text
Commit B
├── index.html
├── style.css
└── app.js (updated)
```

Git can reuse existing objects for unchanged content.

It does not need to create a completely new copy of every unchanged file.

### Snapshot Sharing

```text
                Commit A
                   |
          +--------+--------+
          |        |        |
          v        v        v
        Blob 1   Blob 2   Blob 3
                            |
                            |
                         Commit B
                            |
                   +--------+--------+
                   |        |        |
                   v        v        v
                 Blob 1   Blob 2   Blob 4
```

In this simplified example:

* Blob 1 remains unchanged.
* Blob 2 remains unchanged.
* Blob 3 represents the original `app.js`.
* Blob 4 represents the updated `app.js`.

Unchanged content can be shared between snapshots.

### Why This Matters

This design makes Git:

* Efficient at creating commits.
* Suitable for branching.
* Capable of maintaining historical snapshots.
* Efficient at reusing identical content.

# 🗁 Git Snapshots vs Delta-Based Version Control

Different version control systems use different approaches to store historical changes.

### Delta-Based Storage

A delta-based system can store an original version and subsequent differences.

Example:

```text
Version 1
    |
    v
Difference 1
    |
    v
Version 2
    |
    v
Difference 2
    |
    v
Version 3
```

To reconstruct a later version, the system may need to apply a sequence of differences.

### Snapshot-Based Storage in Git

Git represents commits as project snapshots.

```text
Commit A --> Snapshot A
Commit B --> Snapshot B
Commit C --> Snapshot C
```

Each snapshot is represented by a tree object and references the file contents it contains.

Git can reuse identical objects across snapshots.

Git may also use delta compression internally when storing objects in packfiles.

### Comparison

| Feature              | Delta-Based Model                             | Git Snapshot Model                   |
| -------------------- | --------------------------------------------- | ------------------------------------ |
| Main representation  | Differences                                   | Project snapshots                    |
| Historical states    | Reconstructed from stored versions and deltas | Represented by commits and trees     |
| Storage optimization | Delta storage                                 | Object reuse, compression, packfiles |
| Branching            | Depends on implementation                     | Lightweight references               |
| Git implementation   | Not its primary conceptual model              | Yes                                  |

**Important:** Git's snapshot model is a logical representation, not a claim that every commit duplicates every file.

# 🗁 Git's Internal Data Model

Git internally organizes repository information using objects and references.

The four primary Git object types are:

1. Blob.
2. Tree.
3. Commit.
4. Tag.

These objects are stored in Git's object database.

### Simplified Git Data Model

```text
             Branch Reference
                    |
                    v
                  Commit
                    |
                    v
                   Tree
              /     |     \
             /      |      \
            v       v       v
          Blob    Tree    Blob
                   |
                   v
                  Blob
```

A commit references a root tree.

A tree represents a directory and contains entries for files and subdirectories.

A blob stores file content.

A tag object can provide an annotated reference to another Git object.

### Git Object Database

Git stores its objects inside:

```text
.git/objects/
```

Objects may be stored as loose objects or packed into packfiles.

Git uses object IDs to locate and reference them.

# 🗁 Git Object Types

### Blob Object

A blob stores the content of a file.

It does not store the filename or directory path.

Example:

```text
Hello Git!
```

If two files have identical content, Git can reuse the same blob object for both.

**Important Characteristics**

* Stores file content.
* Does not store filenames.
* Does not store directory structure.
* Identified by an object hash.

### Tree Object

A tree represents a directory.

It contains entries referencing blobs and other trees.

Example:

```text
project/
├── index.html
├── app.js
└── src/
    └── main.js
```

Conceptually:

```text
Root Tree
├── index.html --> Blob
├── app.js     --> Blob
└── src        --> Tree
                   |
                   └── main.js --> Blob
```

A tree stores information such as:

* Entry names.
* File or directory modes.
* Object types.
* Object IDs.

## Commit Object

A commit represents a recorded project snapshot.

A commit object contains:

* Root tree reference.
* Parent commit reference or references.
* Author information.
* Committer information.
* Timestamps.
* Commit message.

Example:

```text
commit abc123...
tree def456...
parent 789abc...
author Developer <developer@example.com>
committer Developer <developer@example.com>

Add authentication feature
```

The above is a simplified representation. The actual object contains structured metadata.

**Parent Relationships**

* A normal commit usually has one parent.
* An initial commit has no parent.
* A merge commit generally has two or more parents.

### Tag Object

Git supports two important types of tags.

**Lightweight tag:**

A simple reference to an object, commonly a commit.

**Annotated tag:**

A Git object containing additional metadata, including:

* Tag name.
* Tagger information.
* Timestamp.
* Message.
* Target object reference.

Annotated tags can also be cryptographically signed.

Example:

```bash
git tag v1.0.0
```

This creates a lightweight tag.

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

This creates an annotated tag.

# 🗁 Git's Content-Addressable Storage

Git uses content-addressable storage to identify objects.

An object ID is derived from a cryptographic hash of the object's type, length, and content.

Conceptually:

```text
Object Type + Size + Content
              |
              v
       Cryptographic Hash
              |
              v
           Object ID
```

For example:

```text
Blob Content:
Hello Git!
```

Git calculates an object ID based on the complete object representation.

Historically, SHA-1 has been the default Git object format in most repositories.

Git also supports SHA-256 repository formats, although interoperability and ecosystem support must be considered when choosing an object format.

### Why Hashing Matters

Hashing provides:

* Object identification.
* Content integrity.
* Duplicate content detection.
* Reliable object references.

### Git Object ID Example

```bash
git hash-object README.md
```

This calculates the object ID for the file content without necessarily storing the object.

To write the object into the repository:

```bash
git hash-object -w README.md
```

The `-w` option writes the object into the Git object database.

### Inspecting Git Objects

```bash
git cat-file -t <object-id>
```

Displays the object type.

```bash
git cat-file -s <object-id>
```

Displays the object size.

```bash
git cat-file -p <object-id>
```

Displays the object content in a human-readable form.

These commands are particularly useful when studying Git internals.

# 🗁 Git Repository States

Git uses different states to describe the relationship between files and the repository.

The most important states are:

1. Untracked.
2. Unmodified.
3. Modified.
4. Staged.

### Untracked

A newly created file that Git is not currently tracking.

Example:

```bash
touch app.js
git status
```

Git reports that `app.js` is untracked.

### Unmodified

A tracked file whose content matches the version in the current commit.

There are no changes to record.

### Modified

A tracked file that has been changed in the working directory but has not yet been staged.

Example:

```bash
echo "console.log('Hello');" > app.js
git status
```

Git identifies the file as modified.

### Staged

A change that has been added to the staging area and is prepared for the next commit.

```bash
git add app.js
```

The change is now staged.

### State Transition

```text
             Untracked
                 |
                 | git add
                 v
               Staged
                 |
                 | git commit
                 v
             Unmodified
                 |
                 | Modify file
                 v
              Modified
                 |
                 | git add
                 v
               Staged
```

A staged file can also be modified again before committing. In that case, Git can show separate staged and unstaged changes for the same file.

# 🗁 Git Working Areas

Git's fundamental workflow involves three local working areas and, optionally, a remote repository.

### Working Directory

The working directory contains the checked-out project files.

You edit files here.

### Staging Area (Index)

The staging area stores the proposed contents of the next commit.

It allows developers to select exactly which changes should be included.

### Local Repository

The local repository contains committed Git objects and references.

Commits are stored in the local repository's object database.

### Remote Repository

A remote repository is another repository that your local repository can communicate with.

It may be hosted on GitHub, GitLab, Bitbucket, or a private Git server.

**Complete Workflow**

```text
+----------------------+
|   Working Directory  |
|                      |
|  Modify/Create Files |
+----------------------+
           |
           | git add
           v
+----------------------+
|    Staging Area      |
|                      |
|  Selected Changes    |
+----------------------+
           |
           | git commit
           v
+----------------------+
|   Local Repository   |
|                      |
|  Commits and History |
+----------------------+
           |
           | git push
           v
+----------------------+
|   Remote Repository  |
|                      |
|  Shared Repository   |
+----------------------+
```

**Important Commands**

| Command             | Purpose                                          |
| ------------------- | ------------------------------------------------ |
| `git status`        | Inspect working tree and staging area            |
| `git add`           | Stage changes                                    |
| `git diff`          | Inspect unstaged changes                         |
| `git diff --cached` | Inspect staged changes                           |
| `git commit`        | Record a commit                                  |
| `git push`          | Send local commits to a remote                   |
| `git fetch`         | Download remote updates without integrating them |
| `git pull`          | Fetch and integrate remote changes               |

# 🗁 Understanding HEAD

HEAD is a special reference that identifies the currently checked-out position in the repository.

In the normal branch-attached state, HEAD points symbolically to the current branch.

Example:

```text
HEAD
 |
 v
main
 |
 v
Commit C
```

When you switch to another branch:

```bash
git switch feature-login
```

HEAD follows the newly checked-out branch.

### HEAD and Detached HEAD

Normally:

```text
HEAD -> main -> Commit C
```

In a detached HEAD state, HEAD points directly to a commit instead of symbolically referencing a branch.

Example:

```bash
git checkout <commit-id>
```

This is useful for inspecting historical commits.

However, commits created while detached may not be referenced by a branch. Create a branch if you want to preserve that work reliably.

### Inspecting HEAD

```bash
git status
```

```bash
git log -1
```

```bash
git rev-parse HEAD
```

```bash
cat .git/HEAD
```

The last command is for examining a typical non-bare repository's HEAD file.

# 🗁 Understanding Git History

Git stores commit history as a directed acyclic graph (DAG).

A DAG consists of:

* Nodes representing commits.
* Directed edges representing parent relationships.
* No directed cycles.

### Linear History

```text
A --> B --> C --> D
```

Each commit has one parent, except the initial commit.

### Branching History

```text
       B --> C
      /       \
A ---          E
      \       /
       D ----
```

This is a simplified graph showing two lines of development that later converge.

### Viewing History

```bash
git log
```

Compact history:

```bash
git log --oneline
```

Graphical history:

```bash
git log --oneline --graph --all
```

Detailed history with references:

```bash
git log --oneline --decorate --graph --all
```

### Commit Relationships

A commit references its parent commit or commits.

This creates a historical relationship between project snapshots.

Git can use these relationships to:

* Determine common ancestors.
* Compare branches.
* Merge development histories.
* Rebase commits.
* Inspect changes between versions.

# 🗁 Git's Fundamental Workflow

A standard Git workflow consists of several stages.

### Step 1: Create a Project

```bash
mkdir git-basics
cd git-basics
```

### Step 2: Initialize Git

```bash
git init
```

This creates a new Git repository.

### Step 3: Create a File

```bash
echo "# Git Basics" > README.md
```

### Step 4: Inspect Repository Status

```bash
git status
```

The new file should appear as untracked.

### Step 5: Stage the File

```bash
git add README.md
```

### Step 6: Create a Commit

```bash
git commit -m "Initial commit"
```

### Step 7: Inspect Commit History

```bash
git log --oneline
```

### Step 8: Modify the File

```bash
echo "Learning Git fundamentals." >> README.md
```

### Step 9: Inspect the Difference

```bash
git diff
```

### Step 10: Stage and Commit Again

```bash
git add README.md

git commit -m "Add Git learning introduction"
```

You now have two commits in your repository history.

# 🗁 Essential Git Commands

This section introduces frequently used Git commands.

| Command         | Description                        |
| --------------- | ---------------------------------- |
| `git --version` | Display Git version                |
| `git help`      | Display Git help                   |
| `git init`      | Initialize a repository            |
| `git clone`     | Clone an existing repository       |
| `git status`    | Display repository status          |
| `git add`       | Stage changes                      |
| `git commit`    | Record changes                     |
| `git log`       | Display commit history             |
| `git diff`      | Compare changes                    |
| `git branch`    | List or manage branches            |
| `git switch`    | Switch branches                    |
| `git merge`     | Merge branches                     |
| `git fetch`     | Fetch remote updates               |
| `git pull`      | Fetch and integrate remote changes |
| `git push`      | Push commits to a remote           |

### Useful Command Combinations

```bash
# Initialize repository
git init

# Check current status
git status

# Stage all changes
git add .

# Create a commit
git commit -m "Add initial implementation"

# View compact history
git log --oneline

# View branch graph
git log --graph --oneline --all

# Compare staged changes
git diff --cached

# Inspect current branch
git branch --show-current
```

# 🗁 Git vs Other Version Control Systems

Git is not the only version control system.

Other systems are used in different development environments.

| Feature | Git | SVN | Mercurial | Perforce |
| --- | --- | --- | --- | --- |
| Architecture | Distributed | Centralized | Distributed | Commonly centralized |
| Offline commits | Yes | No, generally | Yes | Depends on workflow |
| Branching | Lightweight | Supported | Supported | Supported |
| Main use cases | General software development | Centralized project management | Distributed development | Large codebases and game development |
| Repository history | Local clone | Primarily server-side | Local clone | Server-managed |

### When to Use Git

Git is particularly useful for:

* Web development.
* Backend development.
* Frontend development.
* Mobile application development.
* Open-source projects.
* DevOps workflows.
* Infrastructure configuration.
* Team-based software development.

# 🗁 Common Git Terminologies

Understanding Git terminology is essential before learning advanced operations.

| Term | Meaning |
| --- | --- |
| Repository | A Git-managed project and its history |
| Commit | A recorded snapshot of project state |
| Branch | A movable reference to a commit |
| HEAD | Reference to the current checkout position |
| Staging Area | Index containing proposed next-commit content |
| Working Directory | Checked-out project files |
| Blob | Git object storing file content |
| Tree | Git object representing directory structure |
| Tag | Named reference or tag object identifying an important object |
| Remote | Reference to another repository |
| Clone | Local copy of a repository |
| Fetch | Download remote objects and references |
| Pull | Fetch and integrate remote changes |
| Push | Publish local commits to a remote |
| Merge | Integrate histories |
| Rebase | Reapply commits onto a different base |
| Hash | Cryptographic identifier for a Git object |
| Conflict | Situation requiring incompatible changes to be resolved |
| Index | Another name for the staging area |
| DAG | Directed acyclic graph representing commit relationships |
| Checkout | Operation for switching or restoring repository content |

# 🖆 Common Beginner Mistakes

### Thinking Git and GitHub Are the Same

Git is version control software.

GitHub is a hosting and collaboration platform.

### Thinking `git add` Creates a Commit

It does not.

`git add` stages content for the next commit.

`git commit` records that staged content.

### Thinking Every Commit Duplicates the Entire Project

Git represents project snapshots using trees and blobs and reuses identical objects.

### Thinking a Branch Is a Separate Repository

A branch is generally a lightweight reference to a commit.

### Thinking Every Git Command Requires Internet

Most Git operations work locally.

Remote operations require communication with another repository.

### Ignoring Repository Status

Always inspect the repository state before important operations.

```bash
git status
```

### Making Unclear Commit Messages

Avoid:

```bash
git commit -m "update"
git commit -m "changes"
git commit -m "fix"
```

Prefer:

```bash
git commit -m "Add user authentication middleware"
```

A useful commit message describes the purpose of the change.

### Confusing Working Directory and Staging Area

A file can contain staged changes and additional unstaged modifications simultaneously.

Use:

```bash
git diff
```

to inspect unstaged changes.

Use:

```bash
git diff --cached
```

to inspect staged changes.

# 🖆 Best Practices

1. Commit small, logically related changes.
2. Write meaningful commit messages.
3. Inspect changes before committing.
4. Use branches for independent development.
5. Avoid committing generated files and unnecessary dependencies.
6. Configure `.gitignore` appropriately.
7. Keep important repository history understandable.
8. Pull or fetch remote changes regularly when collaborating.
9. Understand destructive commands before executing them.
10. Use tags for important release versions.
11. Prefer explicit file staging when precision is important.
12. Never commit passwords, API keys, or other secrets.

**Example:** `.gitignore`

```gitignore
# Dependencies
node_modules/

# Environment variables
.env
.env.*

# Build output
dist/
build/

# Logs
*.log

# Operating system files
.DS_Store
Thumbs.db

# Editor configuration
.vscode/
```

Adjust ignore patterns according to the project and team requirements.

# 🖆 Hands-on Practical Exercises

The following exercises will help you understand the fundamentals through actual Git operations.

### Exercise 1: Initialize a Repository

**Objective:** Create your first Git repository.

```bash
mkdir git-exercise-01
cd git-exercise-01

git init

git status
```

Expected learning:

* Repository initialization.
* The `.git` directory.
* Initial repository status.

### Exercise 2: Create and Track Files

**Objective:** Understand untracked and staged states.

```bash
echo "# My Git Project" > README.md

git status

git add README.md

git status
```

Expected learning:

* Untracked files.
* Staging area.
* Tracking files.

### Exercise 3: Create Your First Commit

**Objective:** Record project history.

```bash
git commit -m "Initial project setup"

git log --oneline
```

Expected learning:

* Commits.
* Commit messages.
* Commit history.

### Exercise 4: Understand File Modifications

**Objective:** Understand modified and staged states.

```bash
echo "Git is a distributed version control system." >> README.md

git status

git diff

git add README.md

git diff --cached

git commit -m "Add Git introduction"
```

Expected learning:

* Modified files.
* Unstaged changes.
* Staged changes.
* Commit creation.

### Exercise 5: Explore Git Objects

**Objective:** Inspect Git's internal object database.

```bash
# Calculate a blob object ID
git hash-object README.md

# Calculate and store a blob
git hash-object -w README.md
```

Find the object ID and inspect it:

```bash
git cat-file -t <object-id>

git cat-file -p <object-id>

git cat-file -s <object-id>
```

Expected learning:

* Blob objects.
* Object IDs.
* Content-addressable storage.

### Exercise 6: Inspect Repository Internals

**Objective:** Explore the `.git` directory.

```bash
ls -la .git

ls -la .git/objects

cat .git/HEAD
```

Expected learning:

* Repository metadata.
* Object storage.
* HEAD reference.

### Exercise 7: Explore Commit History

**Objective:** Understand commit relationships.

```bash
git log

git log --oneline

git log --graph --oneline --all
```

Expected learning:

* Commit history.
* Parent relationships.
* Graph visualization.

### Exercise 8: Understand Snapshot Reuse

**Objective:** Observe how unchanged content is reused.

1. Create two files.
2. Commit both files.
3. Modify only one file.
4. Create another commit.
5. Inspect the trees and blobs using Git object commands.

Useful commands:

```bash
git ls-tree HEAD

git ls-tree HEAD^

git cat-file -p HEAD

git cat-file -p HEAD^{tree}
```

Expected learning:

* Tree objects.
* Blob references.
* Commit snapshots.
* Reuse of identical content.

# 🖆 Interview Questions

### Beginner Level

**1. What is version control?**

Version control is a system that records changes to files over time and allows developers to inspect, compare, and restore previous versions.

**2. What is Git?**

Git is a distributed version control system used to track changes and manage project history.

**3. Who created Git?**

Linus Torvalds created Git in 2005.

**4. Is Git centralized or distributed?**

Git is a distributed version control system.

**5. What is a repository?**

A repository is a Git-managed project containing tracked content, objects, references, and project history.

**6. What is a commit?**

A commit is a recorded snapshot of the repository's tracked state, together with metadata and references to its parent commit or commits.

**7. What is the staging area?**

The staging area is an index that holds the proposed content of the next commit.

**8. What is the difference between Git and GitHub?**

Git is a version control system, whereas GitHub is a platform for hosting Git repositories and collaborating on projects.

### Intermediate Level

**9. What are the three major types of version control systems?**

* Local VCS.
* Centralized VCS.
* Distributed VCS.

**10. What is a blob in Git?**

A blob is a Git object that stores file content without storing its filename or directory path.

**11. What is a tree object?**

A tree object represents a directory and references blobs or other tree objects.

**12. What is a commit object?**

A commit object contains a root tree reference, parent information, author and committer metadata, timestamps, and a commit message.

**13. What is HEAD in Git?**

HEAD identifies the currently checked-out position, usually through a symbolic reference to the current branch.

**14. What is a detached HEAD state?**

A detached HEAD state occurs when HEAD points directly to a commit instead of referencing a branch.

**15. What is a DAG?**

A Directed Acyclic Graph is a graph structure in which directed relationships do not form cycles. Git uses a DAG to represent commit history.

### Advanced Level

**16. Does Git store complete copies of every file in every commit?**

No. Git represents snapshots using tree and blob objects and reuses identical objects across commits.

**17. What is content-addressable storage?**

It is a storage mechanism in which objects are identified by hashes derived from their content and object metadata.

**18. What is the difference between a blob and a tree?**

A blob stores file content, while a tree stores directory entries and references to blobs or other trees.

**19. Why is branching fast in Git?**

A branch is generally just a lightweight reference pointing to a commit. Creating one does not require copying the entire repository.

**20. What happens when a file is modified after staging?**

The staged version remains in the index, while the newer modification exists in the working directory. Git can therefore show both staged and unstaged changes.

**21. Does Git require an internet connection?**

No. Most Git operations are local. Network connectivity is needed when communicating with remote repositories.

**22. What is the difference between a lightweight tag and an annotated tag?**

A lightweight tag is a simple reference to an object. An annotated tag is a Git object containing metadata and a message, and it can be signed.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official Documentation**

* Git Official Website: <https://git-scm.com/>
* Git Documentation: <https://git-scm.com/docs>
* Git Reference: <https://git-scm.com/docs/git>
* Git Objects: <https://git-scm.com/book/en/v2/Git-Internals-Git-Objects>
* Git Internals: <https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain>
