# Git Tags

Git tags are references used to mark specific points in a repository's history.

They are commonly used to identify important commits, especially software releases.

For example:

```text
v1.0.0
v1.1.0
v1.2.0
v2.0.0
```

A tag allows developers to identify a particular version of their project without having to remember its commit hash.

Example:

```bash
git tag v1.0.0
```

This creates a tag named `v1.0.0` pointing to the current commit.

# 🗁 Understanding Git Tags

A Git tag is a named reference to a specific Git object, most commonly a commit.

Unlike branches, tags are generally intended to remain fixed after creation.

Example Git history:

```text
A --- B --- C --- D --- E
          |       |
        v1.0.0  v1.1.0
```

In this example:

* `v1.0.0` identifies commit `C`.
* `v1.1.0` identifies commit `D`.
* The branch can continue moving forward.
* The tags continue identifying their original release points.

# 🗁 Why Use Git Tags?

Tags are useful for:

* Marking software releases.
* Identifying stable versions.
* Tracking deployment versions.
* Creating release checkpoints.
* Supporting CI/CD pipelines.
* Generating release notes.
* Identifying production deployments.
* Checking out previous versions.

Example:

```bash
git tag v1.0.0
git push origin v1.0.0
```

Now the remote repository can reference the release using `v1.0.0`.

# 🗁 Tags vs Branches

Tags and branches are both Git references, but they serve different purposes.

| Feature | Branch | Tag |
| --- | --- | --- |
| Purpose | Ongoing development | Marking a specific point |
| Moves automatically | Yes, when new commits are added | No |
| New commits | Commonly added | Not directly |
| Common usage | Feature development | Software releases |
| Example | `main` | `v1.0.0` |

Example:

```bash
# Create a branch
git branch feature/authentication

# Create a tag
git tag v1.0.0
```

A branch moves when new commits are made on it. A tag normally remains associated with the same target.

# 🗁 Types of Git Tags

Git supports two main types of tags:

* Lightweight tags
* Annotated tags

### 🖆 Lightweight Tags

A lightweight tag is simply a reference to a commit.

It does not create a separate tag object.

Create a lightweight tag:

```bash
git tag v1.0.0
```

Inspect it:

```bash
git show v1.0.0
```

Lightweight tags are useful for temporary or personal checkpoints.

### 🖆 Annotated Tags

An annotated tag is a complete Git object containing metadata.

It includes:

* Tag name
* Tagger information
* Creation date
* Tag message
* Target object
* Optional cryptographic signature

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Inspect it:

```bash
git show v1.0.0
```

Annotated tags are generally recommended for official software releases because they provide release metadata.

### 🖆 Lightweight vs Annotated Tags

| Feature | Lightweight | Annotated |
| --- | --- | --- |
| Git object | No separate tag object | Creates a tag object |
| Metadata | No tag metadata | Includes tag metadata |
| Message | No | Yes |
| Tagger information | No | Yes |
| Digital signature | No | Yes |
| Release usage | Less common | Recommended |

# 🗁 Creating Tags

### 🖆 Create a Lightweight Tag

```bash
git tag v1.0.0
```

This tags the current commit.

### 🖆 Create an Annotated Tag

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

### 🖆 Create a Tag With an Editor Message

```bash
git tag -a v1.0.0
```

Git opens your configured editor so you can write a detailed tag message.

### 🖆 Create a Tag With a Different Name

```bash
git tag release-2026-01
```

### 🖆 Create a Tag on a Specific Commit

```bash
git tag -a v1.0.0 <commit-hash> -m "Release version 1.0.0"
```

Replace `<commit-hash>` with the actual commit hash.

# 🗁 Tag Naming Conventions

Use meaningful and consistent tag names.

Common conventions:

```text
v1.0.0
v1.1.0
v1.2.0
v2.0.0

release-2026-01
release-2026-02

beta-1.0.0
rc-1.0.0
```

For software releases, the `vMAJOR.MINOR.PATCH` convention is widely used.

Examples:

```text
v1.0.0
v1.0.1
v1.1.0
v2.0.0
```

The `v` prefix is a convention, not a Git requirement.

# 🗁 Semantic Versioning

Semantic Versioning (SemVer) is a versioning convention used to communicate changes between software releases.

Format:

```text
MAJOR.MINOR.PATCH
```

### 🖆 MAJOR

Increment the major version when making incompatible API changes.

Example:

```text
1.0.0 → 2.0.0
```

### 🖆 MINOR

Increment the minor version when adding functionality in a backward-compatible manner.

Example:

```text
1.0.0 → 1.1.0
```

### 🖆 PATCH

Increment the patch version when fixing backward-compatible bugs.

Example:

```text
1.0.0 → 1.0.1
```

### 🖆 Pre-release Versions

Pre-release versions can be identified using a hyphen.

Examples:

```text
1.0.0-alpha
1.0.0-alpha.1
1.0.0-beta
1.0.0-rc.1
```

These versions indicate that a release is not yet considered final.

### 🖆 Build Metadata

Build metadata can be added using a plus sign.

Examples:

```text
1.0.0+build.5
1.0.0+20261107
```

Build metadata does not affect semantic version precedence.

# 🗁 Listing Tags

### 🖆 List All Tags

```bash
git tag
```

### 🖆 List Tags Matching a Pattern

```bash
git tag --list "v1.*"
```

Example output:

```text
v1.0.0
v1.0.1
v1.1.0
v1.2.0
```

### 🖆 Sort Tags by Version

```bash
git tag --sort=-version:refname
```

### 🖆 Sort Tags by Creation Date

```bash
git tag --sort=-creatordate
```

### 🖆 Search for Specific Release Tags

```bash
git tag --list "v2.*"
```

# 🗁 Inspecting Tags

### 🖆 Show Tag Information

```bash
git show v1.0.0
```

For annotated tags, this displays tag metadata and the target commit information.

### 🖆 Show the Commit Referenced by a Tag

```bash
git rev-parse v1.0.0
```

### 🖆 Check the Object Type

```bash
git cat-file -t v1.0.0
```

Possible output:

For an annotated tag:

```text
tag
```

For a lightweight tag:

```text
commit
```

### 🖆 Show Tag References

```bash
git show-ref --tags
```

### 🖆 List Tag References With Details

```bash
git for-each-ref refs/tags
```

# 🗁 Tagging Previous Commits

You can create a tag for any existing commit, not just the current commit.

Find commit history:

```bash
git log --oneline
```

Example:

```text
a8f34d2 Update authentication
b7e21c9 Add user registration
c5d12a8 Initial project setup
```

Create a tag for an older commit:

```bash
git tag -a v1.0.0 c5d12a8 -m "Initial stable release"
```

Verify:

```bash
git show v1.0.0
```

This is useful when you need to mark a previous commit as an official release.

# 🗁 Pushing Tags to Remote

Unlike ordinary branch updates, tags are not normally pushed automatically by a standard `git push`.

### 🖆 Push a Specific Tag

```bash
git push origin v1.0.0
```

### 🖆 Push All Local Tags

```bash
git push origin --tags
```

This pushes all local tags that are not already present on the remote.

### 🖆 Push Reachable Annotated Tags

```bash
git push origin --follow-tags
```

This pushes annotated tags reachable from the commits being pushed, provided they are not already present on the remote.

Unlike `--tags`, this does not push every local tag.

### 🖆 Verify Remote Tags

```bash
git ls-remote --tags origin
```

# 🗁 Fetching Remote Tags

Tags from a remote repository can be retrieved using Git fetch.

### 🖆 Fetch Tags

```bash
git fetch --tags
```

This fetches tags from the configured remote, including tags that may not be reachable from the fetched branch tips.

### 🖆 Fetch From a Specific Remote

```bash
git fetch origin --tags
```

### 🖆 Fetch a Specific Tag

```bash
git fetch origin tag v1.0.0
```

### 🖆 List Available Remote Tags

```bash
git ls-remote --tags origin
```

# 🗁 Deleting Tags

Git supports deleting tags locally and remotely.

### 🖆 Delete a Local Tag

```bash
git tag -d v1.0.0
```

### 🖆 Delete a Remote Tag

```bash
git push origin --delete v1.0.0
```

An older equivalent syntax is:

```bash
git push origin :refs/tags/v1.0.0
```

### 🖆 Delete Both Local and Remote Tags

```bash
# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
```

Deleting a remote tag affects everyone using that repository. Coordinate with collaborators before removing published release tags.

# 🗁 Renaming Tags

Git does not provide a dedicated tag rename command.

To rename a tag, delete the old reference and create a new one pointing to the same commit.

Example:

```bash
# Get the commit referenced by the old tag
git rev-parse v1.0.0

# Create a new tag pointing to the same commit
git tag -a release-v1.0.0 <commit-hash> -m "Release version 1.0.0"

# Delete the old local tag
git tag -d v1.0.0
```

If the old tag was published, update the remote references too:

```bash
git push origin --delete v1.0.0
git push origin release-v1.0.0
```

Avoid changing published release tags unless necessary.

# 🗁 Checking Out a Tag

You can check out a tag to inspect the project at a specific release.

```bash
git switch --detach v1.0.0
```

This places HEAD in a detached state.

### 🖆 Understanding Detached HEAD

In detached HEAD state, HEAD points directly to a commit rather than a branch.

You can inspect files and run the application, but new commits will not automatically belong to a branch.

### 🖆 Create a Branch From a Tag

If you want to make changes based on a tagged release:

```bash
git switch -c hotfix/v1.0.1 v1.0.0
```

This creates a new branch starting from the commit referenced by `v1.0.0`.

# 🗁 Comparing Tags

Tags can be used to compare different releases.

### 🖆 Compare Two Tags

```bash
git diff v1.0.0..v1.1.0
```

This displays the file changes between the two tagged commits.

### 🖆 View Commits Between Tags

```bash
git log v1.0.0..v1.1.0 --oneline
```

### 🖆 Show Files Changed Between Releases

```bash
git diff --stat v1.0.0..v1.1.0
```

### 🖆 Show Changed File Names

```bash
git diff --name-only v1.0.0..v1.1.0
```

These commands are useful when preparing release notes or reviewing changes between versions.

# 🗁 Signed Tags

Git supports cryptographically signed annotated tags.

Signed tags help verify the authenticity and integrity of release tags.

### 🖆 Create a Signed Tag

```bash
git tag -s v1.0.0 -m "Signed release version 1.0.0"
```

This requires a configured signing key, commonly a GPG key.

### 🖆 Verify a Signed Tag

```bash
git tag -v v1.0.0
```

Git checks the tag signature against the available public key.

### 🖆 Create a Signed Tag With a Signing Key

```bash
git tag -s v1.0.0 -u <key-id> -m "Release version 1.0.0"
```

Signed tags are useful when release authenticity and provenance matter.

# 🗁 Git Describe

`git describe` generates a human-readable description of a commit based on the nearest reachable tag.

Command:

```bash
git describe
```

Example output:

```text
v1.0.0-5-gabc1234
```

Meaning:

* `v1.0.0`: nearest reachable tag.
* `5`: number of commits after that tag.
* `gabc1234`: abbreviated commit hash.

Include lightweight tags in the search:

```bash
git describe --tags
```

Find a description for any commit:

```bash
git describe --tags <commit-hash>
```

This can be useful in build systems and CI/CD pipelines for generating version information.

# 🗁 How Git Stores Tags

Git stores tag references under:

```text
.git/refs/tags/
```

Example:

```text
.git/
├── HEAD
├── config
├── objects/
├── refs/
│   ├── heads/
│   │   ├── main
│   │   └── develop
│   ├── remotes/
│   └── tags/
│       ├── v1.0.0
│       └── v1.1.0
```

Each tag reference contains an object ID.

### 🖆 Lightweight Tag Internals

A lightweight tag points directly to a commit object.

```text
refs/tags/v1.0.0
       |
       v
    Commit
```

### 🖆 Annotated Tag Internals

An annotated tag points to a tag object, which references its target commit.

```text
refs/tags/v1.0.0
       |
       v
    Tag Object
       |
       v
     Commit
```

The tag object contains metadata such as the tagger, message, and optional signature.

Inspect an annotated tag object:

```bash
git cat-file -p v1.0.0
```

# 🗁 Practical Release Workflow

A typical Git release workflow uses annotated tags to mark stable versions.

### 🖆 Example Workflow

Start by checking the working tree:

```bash
git status
```

Review the latest commits:

```bash
git log --oneline -5
```

Make sure the release changes are committed:

```bash
git add .
git commit -m "Prepare version 1.0.0 release"
```

Create an annotated release tag:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Push the branch:

```bash
git push origin main
```

Push the release tag:

```bash
git push origin v1.0.0
```

Verify the remote tag:

```bash
git ls-remote --tags origin
```

The release tag can now be used by deployment pipelines and release-management tools.

### 🖆 Example Version Progression

```text
Initial release:
v1.0.0

Bug fix:
v1.0.1

New backward-compatible feature:
v1.1.0

Breaking API change:
v2.0.0
```

# 🖆 Best Practices

* Prefer annotated tags for official releases.
* Use semantic versioning consistently.
* Use meaningful and predictable tag names.
* Create release tags only after validating the release commit.
* Push tags explicitly when publishing releases.
* Treat published release tags as immutable.
* Verify tags before deploying.
* Use signed tags when release authenticity is important.
* Keep release notes associated with version tags.
* Avoid deleting or moving tags that other developers or deployment systems depend on.

# 🖆 Common Problems and Solutions

### 🖆 Tag Already Exists

Error:

```text
fatal: tag 'v1.0.0' already exists
```

Solution:

```bash
git tag
git tag -d v1.0.0
```

Only recreate the tag if changing its target is intentional and coordinated.

### 🖆 Tag Not Available on Remote

Check local tags:

```bash
git tag
```

Push the required tag:

```bash
git push origin v1.0.0
```

### 🖆 Remote Tag Does Not Match Local Tag

Compare references:

```bash
git rev-parse v1.0.0
git ls-remote origin refs/tags/v1.0.0
```

For annotated tags, the remote tag reference points to the tag object, so its object ID may differ from the commit ID shown by `git rev-parse`.

To compare the target commit, inspect the tag locally and fetch the remote tag if necessary.

### 🖆 Cannot Commit While on a Tag

You may be in detached HEAD state.

Create a branch:

```bash
git switch -c hotfix-branch
```

### 🖆 Tag Does Not Appear After Fetch

Fetch tags explicitly:

```bash
git fetch origin --tags
```

Then list them:

```bash
git tag
```

# 🖆 Practical Exercises

* Create a lightweight tag named `v1.0.0`.
* Create an annotated tag named `v1.1.0`.
* List all tags in the repository.
* Filter tags using `v1.*`.
* Inspect a tag using `git show`.
* Identify whether a tag is lightweight or annotated.
* Tag an older commit using its commit hash.
* Push a specific tag to GitHub.
* Fetch tags from a remote repository.
* Compare two tags using `git diff`.
* Create a branch from an existing tag.
* Delete a local tag.
* Delete a remote tag.
* Create and verify a signed tag if you have a signing key configured.

# 🖆 Interview Questions

**What is a Git tag?**

A Git tag is a reference used to mark a specific object, commonly a commit, in repository history.

**What are the two main types of Git tags?**

Lightweight tags and annotated tags.

**What is the difference between a lightweight and annotated tag?**

A lightweight tag is a simple reference to an object. An annotated tag creates a Git object containing metadata and an optional signature.

**Which tag type is recommended for software releases?**

Annotated tags, because they contain release metadata and can be signed.

**How do you create an annotated tag?**

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

**How do you push a specific tag to GitHub?**

```bash
git push origin v1.0.0
```

**How do you push all local tags?**

```bash
git push origin --tags
```

**How do you delete a local tag?**

```bash
git tag -d v1.0.0
```

**How do you delete a remote tag?**

```bash
git push origin --delete v1.0.0
```

**Can you create a tag for an older commit?**

Yes.

```bash
git tag -a v1.0.0 <commit-hash> -m "Release version 1.0.0"
```

**What is semantic versioning?**

A versioning convention using MAJOR.MINOR.PATCH to communicate the type and compatibility of software changes.

**What happens when you check out a tag?**

Git normally enters detached HEAD state because HEAD points directly to the tagged commit rather than a branch.

**How do you create a branch from a tag?**

```bash
git switch -c new-branch v1.0.0
```

**What does `git describe` do?**

It generates a human-readable description of a commit based on the nearest reachable tag.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [Git Documentation — git-tag](https://git-scm.com/docs/git-tag)
* [Git Documentation — git-push](https://git-scm.com/docs/git-push)
* [Git Documentation — git-fetch](https://git-scm.com/docs/git-fetch)
* [Git Documentation — git-describe](https://git-scm.com/docs/git-describe)
* [Git Documentation — git-checkout](https://git-scm.com/docs/git-checkout)
* [Semantic Versioning](https://semver.org/)
