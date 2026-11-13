# GitHub Releases

GitHub Releases provide a way to package, publish, and distribute specific versions of a software project.

They allow developers to publish stable versions of their applications along with:

* Version numbers.
* Release notes.
* Source code archives.
* Compiled binaries.
* Installation packages.
* Documentation.
* Other downloadable assets.

GitHub Releases are built around Git tags.

For example, when an application reaches version `v1.0.0`, you can create a Git tag for that version and publish a GitHub Release associated with it.

# 🗁 What Is a GitHub Release?

A **GitHub Release** is a published version of a software project associated with a Git tag.

It represents a specific point in the repository's history.

For example:

```text
v1.0.0
v1.1.0
v1.2.0
v2.0.0
```

Each release can contain:

* A tag identifying the version.
* A release title.
* Release notes.
* Source code archives.
* Downloadable assets.
* Publication information.

Example:

```text
Project: My Application

Release: v1.2.0

Changes:
- Added user authentication.
- Improved API performance.
- Fixed login validation.
- Updated documentation.
```

Users can visit the repository's **Releases** section to view and download published versions.

# 🗁 Why Use GitHub Releases?

GitHub Releases make software distribution and version management easier.

* **Version Distribution:** Users can download a specific version without manually checking out a branch.
* **Release Documentation:** Release notes explain what changed between versions.
* **Binary Distribution:** Developers can upload compiled applications and installation packages.
* **Version Tracking:** Git tags provide stable references to specific commits.
* **Project History:** Users can inspect previous releases and understand how a project has evolved.
* **Automated Publishing:** GitHub Actions can automatically build, package, and publish releases.

# 🗁 Git Tag vs GitHub Release

Git tags and GitHub Releases are related, but they are not the same thing.

### 🖆 Git Tag

A Git tag is a reference to a specific commit.

Example:

```bash
git tag v1.0.0
```

Push the tag:

```bash
git push origin v1.0.0
```

The tag identifies a specific point in Git history.

### 🖆 GitHub Release

A GitHub Release is a GitHub feature built around a tag.

It adds:

* Release title.
* Release notes.
* Downloadable assets.
* Draft and pre-release states.
* Publication information.

### 🖆 Comparison

| Feature | Git Tag | GitHub Release |
| --- | --- | --- |
| Identifies a commit | Yes | Through its associated tag |
| Exists in Git history | Yes | No, it is GitHub release metadata |
| Release notes | Optional tag annotation | Yes |
| Downloadable assets | No release asset management | Yes |
| Draft release | No | Yes |
| Pre-release status | No | Yes |
| Created using Git | Yes | No, requires GitHub or an API/CLI workflow |

A Git tag can exist without a GitHub Release.

A GitHub Release is associated with a tag, whether the tag already exists or is created during release creation.

# 🗁 Understanding Semantic Versioning

Semantic Versioning (SemVer) is a version-numbering convention used to communicate the nature of software changes.

The standard format is:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
2.4.1
```

| Component | Meaning |
| --- | --- |
| MAJOR | Incompatible API changes |
| MINOR | Backward-compatible new functionality |
| PATCH | Backward-compatible bug fixes |

### 🖆 MAJOR Version

Increase the major version when introducing incompatible changes.

Example:

```text
1.5.2 → 2.0.0
```

### 🖆 MINOR Version

Increase the minor version when adding backward-compatible functionality.

Example:

```text
1.5.2 → 1.6.0
```

The patch version resets to zero.

### 🖆 PATCH Version

Increase the patch version when fixing bugs without changing existing functionality incompatibly.

Example:

```text
1.5.2 → 1.5.3
```

### 🖆 Pre-Release Versions

Semantic Versioning supports pre-release identifiers.

Examples:

```text
1.0.0-alpha
1.0.0-beta
1.0.0-rc.1
```

These versions indicate that the software is not yet a final stable release.

### 🖆 Build Metadata

Build metadata can be added using a plus sign:

```text
1.0.0+build.25
```

Build metadata does not affect version precedence under SemVer.

### 🖆 Version Progression Example

```text
1.0.0
  |
  +-- 1.0.1  Bug fix
  |
  +-- 1.1.0  New feature
  |
  +-- 1.2.0  Another feature
  |
  +-- 2.0.0  Breaking changes
```

Semantic Versioning is especially useful for libraries, APIs, and applications distributed to external users.

# 🗁 Release Lifecycle

A typical release process looks like this:

```text
Development
    |
    v
Feature Completion
    |
    v
Testing and Validation
    |
    v
Version Selection
    |
    v
Create Git Tag
    |
    v
Build Release Artifacts
    |
    v
Write Release Notes
    |
    v
Publish GitHub Release
    |
    v
Distribute to Users
```

A release may also go through pre-release stages:

```text
Development
    |
    v
Alpha
    |
    v
Beta
    |
    v
Release Candidate
    |
    v
Stable Release
```

These stages are common release-management practices, not mandatory GitHub states.

# 🗁 Creating a Release

You can create a GitHub Release through the web interface.

### 🖆 Prepare the Repository

Make sure your changes are committed and pushed.

```bash
git status

git add .

git commit -m "Prepare version 1.0.0"

git push origin main
```

### 🖆 Create a Git Tag

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Annotated tags contain metadata such as the tagger, date, and message.

Push the tag to GitHub:

```bash
git push origin v1.0.0
```

### 🖆 Open the Releases Section

Navigate to your GitHub repository.

Select:

**Releases → Create a new release**

### 🖆 Configure the Release

Provide:

* Tag: `v1.0.0`
* Target: `main`
* Release title: `Version 1.0.0`
* Description: Release notes.

You can also attach downloadable assets.

Select **Publish release** when the release is ready.

# 🗁 Creating a Release with GitHub CLI

GitHub CLI provides the `gh release` command for managing releases.

Authenticate first:

```bash
gh auth login
```

Create a Git tag:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"

git push origin v1.0.0
```

Create a GitHub Release:

```bash
gh release create v1.0.0
```

Create a release with a title and notes:

```bash
gh release create v1.0.0 \
  --title "Version 1.0.0" \
  --notes "Initial stable release."
```

Create a release using a notes file:

```bash
gh release create v1.0.0 \
  --title "Version 1.0.0" \
  --notes-file RELEASE_NOTES.md
```

If the tag does not exist, GitHub CLI can create it using the selected target commit or branch.

You can specify a target:

```bash
gh release create v1.0.0 \
  --target main \
  --title "Version 1.0.0" \
  --notes "Initial release."
```

# 🗁 Release Tags

A release tag identifies the commit associated with a release.

Common tag naming conventions:

```text
v1.0.0
v1.2.0
v2.0.0
```

Some projects use tags without the `v` prefix:

```text
1.0.0
1.2.0
2.0.0
```

Choose a consistent convention for your project.

### 🖆 Create an Annotated Tag

```bash
git tag -a v1.0.0 -m "Version 1.0.0"
```

### 🖆 Create a Lightweight Tag

```bash
git tag v1.0.0
```

### 🖆 List Tags

```bash
git tag
```

### 🖆 Inspect a Tag

```bash
git show v1.0.0
```

### 🖆 Push a Specific Tag

```bash
git push origin v1.0.0
```

### 🖆 Push All Tags

```bash
git push origin --tags
```

### 🖆 Delete a Local Tag

```bash
git tag -d v1.0.0
```

### 🖆 Delete a Remote Tag

```bash
git push origin --delete v1.0.0
```

**Important:** Avoid deleting or moving published release tags unless there is a strong reason. Consumers may already depend on the original tag.

# 🗁 Release Titles and Descriptions

A release title should clearly identify the version.

Examples:

```text
Version 1.0.0
Version 1.2.0 - Authentication Improvements
Version 2.0.0 - Major API Update
```

Release notes should explain the changes included in the release.

A useful structure:

```markdown
# Version 1.0.0

# 🗁 Features

- Added user authentication.
- Added API rate limiting.

# 🗁 Improvements

- Improved database query performance.
- Updated application documentation.

# 🗁 Bug Fixes

- Fixed login validation.
- Fixed incorrect API responses.

# 🗁 Breaking Changes

- Updated authentication response format.

# 🗁 Installation

Download the release package and follow the installation instructions.
```

Good release notes help users understand whether they should upgrade and whether the new version introduces breaking changes.

# 🗁 Automatically Generated Release Notes

GitHub can generate release notes using merged pull requests and contributor information.

When creating a release, select **Generate release notes**.

GitHub can include:

* Merged pull requests.
* Contributors.
* Categorized changes.
* Relevant release information.

You can also generate notes using GitHub CLI:

```bash
gh release create v1.1.0 --generate-notes
```

Generate notes and provide a custom title:

```bash
gh release create v1.1.0 \
  --title "Version 1.1.0" \
  --generate-notes
```

Automatically generated notes are useful, but review them before publishing.

You may need to improve descriptions, remove irrelevant changes, or add migration instructions.

# 🗁 Release Assets

Release assets are downloadable files attached to a GitHub Release.

Common assets include:

* `.zip` archives.
* `.tar.gz` packages.
* `.exe` installers.
* `.deb` packages.
* `.rpm` packages.
* Compiled binaries.
* Application installers.
* Checksums.
* Documentation archives.

Example:

```text
Release v1.0.0
│
├── application-linux-amd64.tar.gz
├── application-windows-x64.zip
├── application-macos-arm64.tar.gz
└── checksums.txt
```

### 🖆 Upload Assets with GitHub CLI

```bash
gh release create v1.0.0 \
  application-linux-amd64.tar.gz \
  application-windows-x64.zip \
  checksums.txt
```

Upload files to an existing release:

```bash
gh release upload v1.0.0 \
  application-linux-amd64.tar.gz \
  checksums.txt
```

### 🖆 Replace Existing Assets

```bash
gh release upload v1.0.0 \
  application-linux-amd64.tar.gz \
  --clobber
```

The `--clobber` option replaces an existing asset with the same name.

### 🖆 Generate Checksums

For Linux:

```bash
sha256sum application-linux-amd64.tar.gz
```

For macOS:

```bash
shasum -a 256 application-macos-arm64.tar.gz
```

Checksums help users verify that downloaded files match the published files.

For stronger supply-chain assurance, publish checksums through a trusted release process and consider artifact signing and provenance.

# 🗁 Draft Releases

A draft release is an unpublished release that is still being prepared.

Draft releases are useful when:

* Release notes are incomplete.
* Assets are still being uploaded.
* Testing is in progress.
* Maintainers need to review the release before publication.

Create a draft release with GitHub CLI:

```bash
gh release create v1.0.0 --draft
```

Publish it later through GitHub or GitHub CLI.

Draft releases are not publicly published as normal releases until they are published.

# 🗁 Pre-Releases

A pre-release is a release intended for testing or evaluation before a stable release.

Examples:

```text
v2.0.0-alpha.1
v2.0.0-beta.1
v2.0.0-rc.1
```

Create a pre-release:

```bash
gh release create v2.0.0-beta.1 \
  --title "Version 2.0.0 Beta 1" \
  --prerelease \
  --notes "First beta release for version 2.0.0."
```

Pre-releases are useful for:

* Beta testing.
* Early access.
* Compatibility testing.
* Collecting user feedback.
* Testing breaking changes.

A pre-release is not necessarily production-ready.

# 🗁 Latest Release

GitHub identifies a release as the latest release for a repository.

The latest release is generally determined using GitHub's release ordering and publication metadata, with special behavior for pre-releases and releases explicitly marked as latest.

When publishing a stable release, verify that GitHub identifies the intended version as the latest release.

Using GitHub CLI, you can mark a release as the latest:

```bash
gh release edit v1.0.0 --latest
```

You can also explicitly mark a release as not latest:

```bash
gh release edit v1.0.0 --latest=false
```

This is useful when managing stable releases alongside pre-releases or maintenance releases.

# 🗁 Release Management

GitHub provides several commands for inspecting and managing releases.

### 🖆 List Releases

```bash
gh release list
```

### 🖆 View a Release

```bash
gh release view v1.0.0
```

Open a release in the browser:

```bash
gh release view v1.0.0 --web
```

### 🖆 Download Release Assets

Download all assets:

```bash
gh release download v1.0.0
```

Download a specific file:

```bash
gh release download v1.0.0 \
  --pattern "*.zip"
```

Download assets into a specific directory:

```bash
gh release download v1.0.0 \
  --dir ./downloads
```

### 🖆 Edit a Release

```bash
gh release edit v1.0.0 \
  --title "Version 1.0.0 - Stable"
```

Update release notes:

```bash
gh release edit v1.0.0 \
  --notes "Updated release documentation."
```

### 🖆 Delete a Release

```bash
gh release delete v1.0.0
```

Delete without confirmation:

```bash
gh release delete v1.0.0 --yes
```

Deleting a GitHub Release and deleting its Git tag are separate operations. Deleting a release does not necessarily delete the associated tag.

# 🗁 Versioning Strategies

Different projects may use different versioning strategies.

### 🖆 Semantic Versioning

Commonly used by libraries and APIs.

```text
MAJOR.MINOR.PATCH
```

Example:

```text
1.0.0
1.1.0
1.1.1
2.0.0
```

### 🖆 Calendar Versioning

Some projects use dates as version identifiers.

Example:

```text
2026.01
2026.06
2026.11
```

This is often called Calendar Versioning or CalVer.

### 🖆 Fixed Release Versions

Some applications use predefined release numbers.

Example:

```text
2026.1
2026.2
2026.3
```

The important requirement is consistency and clear communication about compatibility.

# 🗁 Automating Releases with GitHub Actions

GitHub Actions can automate the release process.

A common approach is to trigger a workflow when a version tag is pushed.

### 🖆 Release Workflow

Create:

```text
.github/workflows/release.yml
```

Example:

```yaml
name: Build and Publish Release

on:
  push:
    tags:
      - "v*"

permissions:
  contents: write

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build application
        run: npm run build

      - name: Package build
        run: tar -czf application.tar.gz dist/

      - name: Create GitHub Release
        env:
          GH_TOKEN: ${{ github.token }}
        run: |
          gh release create "${GITHUB_REF_NAME}" \
            application.tar.gz \
            --generate-notes
```

### 🖆 How It Works

When a tag matching `v*` is pushed:

* GitHub starts the workflow.
* The repository is checked out.
* Node.js is configured.
* Dependencies are installed.
* Tests are executed.
* The application is built.
* The build output is packaged.
* A GitHub Release is created.
* The package is uploaded as a release asset.

The workflow uses the automatically provided GitHub token with `contents: write` permission to create the release.

**Important:** The example assumes the project has the specified npm scripts and produces a `dist/` directory.

### 🖆 Trigger the Release Workflow

Create and push a tag:

```bash
git tag -a v1.0.0 -m "Release v1.0.0"

git push origin v1.0.0
```

GitHub Actions will start the workflow.

Inspect the execution in the repository's **Actions** tab.

# 🗁 Release Notes Configuration

GitHub allows repositories to configure automatically generated release notes.

Create:

```text
.github/release.yml
```

Example:

```yaml
changelog:
  categories:
    - title: Features
      labels:
        - feature
        - enhancement

    - title: Bug Fixes
      labels:
        - bug
        - fix

    - title: Documentation
      labels:
        - documentation

    - title: Other Changes
      labels:
        - "*"
```

This configuration helps categorize merged pull requests in automatically generated release notes.

For example:

```text
Features
- Add authentication API
- Add user profile management

Bug Fixes
- Fix login validation

Documentation
- Update installation guide
```

Use consistent labels on pull requests to improve release note organization.

# 🗁 Managing Releases with GitHub CLI

Common release-management commands:

| Command | Purpose |
| --- | --- |
| `gh release list` | List releases |
| `gh release view` | View release details |
| `gh release create` | Create a release |
| `gh release edit` | Edit a release |
| `gh release delete` | Delete a release |
| `gh release download` | Download release assets |
| `gh release upload` | Upload release assets |
| `gh release verify` | Verify supported release attestations |

Examples:

Create a release:

```bash
gh release create v1.0.0
```

Create a draft:

```bash
gh release create v1.0.0 --draft
```

Create a pre-release:

```bash
gh release create v1.0.0-beta.1 --prerelease
```

Generate release notes:

```bash
gh release create v1.1.0 --generate-notes
```

Download a release:

```bash
gh release download v1.0.0
```

List releases:

```bash
gh release list
```

For the exact supported options, consult the GitHub CLI release manual.

# 🗁 Reverting a Release

Sometimes a published release contains a serious bug.

A release can be withdrawn or superseded, but deleting a release does not automatically reverse its code changes.

A safer approach is usually to publish a new corrective release.

Example:

```text
v1.0.0  Problematic release
v1.0.1  Corrected release
```

If you need to reverse the code changes, use Git's revert operation and publish a new version.

For a regular commit:

```bash
git revert <commit-hash>
```

For a merge commit:

```bash
git revert -m 1 <merge-commit-hash>
```

Then create a new release tag:

```bash
git tag -a v1.0.1 -m "Release version 1.0.1"

git push origin v1.0.1
```

Publish the corrective GitHub Release.

**Best practice:** Avoid silently replacing an already published version. Publish a new version so users can clearly identify the correction.

# 🖆 Release Best Practices

### 🖆 Follow a Consistent Versioning Strategy

Choose Semantic Versioning, Calendar Versioning, or another documented convention.

### 🖆 Use Annotated Tags

Annotated tags contain useful metadata and are generally preferable for release tags.

### 🖆 Write Clear Release Notes

Explain features, improvements, bug fixes, and breaking changes.

### 🖆 Test Before Publishing

Run automated tests and validate build outputs before publishing.

### 🖆 Keep Release Assets Organized

Use descriptive filenames that identify:

* Application.
* Operating system.
* Architecture.
* Version.
* Package format.

Example:

```text
myapp-v1.0.0-linux-amd64.tar.gz
myapp-v1.0.0-windows-x64.zip
```

### 🖆 Publish Checksums

Provide SHA-256 checksums for downloadable files.

### 🖆 Use Draft Releases

Prepare release notes and assets before making a release public.

### 🖆 Use Pre-Releases for Testing

Publish alpha, beta, or release-candidate versions before stable releases when appropriate.

### 🖆 Automate Repetitive Tasks

Use GitHub Actions to automate:

* Testing.
* Building.
* Packaging.
* Release creation.
* Asset uploads.

### 🖆 Protect Release Credentials

Use minimal workflow permissions and secure credentials.

### 🖆 Do Not Move Published Tags Casually

Changing an existing release tag can create inconsistencies for users and automation systems.

# 🖆 Common Problems and Troubleshooting

### 🖆 Release Tag Does Not Exist

Check available tags:

```bash
git tag
```

Create the tag:

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
```

Push it:

```bash
git push origin v1.0.0
```

### 🖆 GitHub Actions Release Workflow Does Not Run

Check:

* Workflow file location.
* Tag trigger configuration.
* Whether the tag was pushed.
* Workflow permissions.
* Repository Actions settings.

Verify the tag:

```bash
git ls-remote --tags origin
```

### 🖆 Release Creation Fails Due to Permissions

The workflow may not have permission to create releases.

Configure:

```yaml
permissions:
  contents: write
```

Also verify repository and organization workflow permission settings.

### 🖆 Release Asset Upload Fails

Possible causes:

* File does not exist.
* Incorrect file path.
* Asset name already exists.
* Insufficient permissions.
* Upload size or service restrictions.

Verify the file:

```bash
ls -lh application.tar.gz
```

### 🖆 Incorrect Release Notes

Review:

* Pull request titles.
* Pull request labels.
* Release configuration.
* Generated notes.

You can edit the release notes before or after publication.

### 🖆 Wrong Version Is Marked Latest

Inspect the release list and explicitly set the intended latest release:

```bash
gh release edit v1.0.0 --latest
```

### 🖆 Release Was Created with the Wrong Tag

Review the release and tag carefully.

If the release is not yet distributed, you may delete and recreate it with the correct tag.

For a published release, consider the impact on users before changing or deleting its associated tag.

# 🖆 Practical Exercises

### 🖆 Exercise: Create Your First Release

* Create a GitHub repository.
* Commit a project version.
* Create an annotated Git tag.
* Push the tag.
* Create a GitHub Release.
* Write release notes.
* Publish the release.

### 🖆 Exercise: Practice Semantic Versioning

Create a version sequence:

```text
v1.0.0
v1.0.1
v1.1.0
v2.0.0
```

Explain why each version changes.

### 🖆 Exercise: Upload Release Assets

* Create a sample application archive.
* Create a GitHub Release.
* Upload the archive.
* Download it using GitHub CLI.
* Verify its contents.

### 🖆 Exercise: Create a Pre-Release

* Create a beta version tag.
* Publish a pre-release.
* Add testing instructions.
* Download and test the release.
* Publish a stable version afterward.

### 🖆 Exercise: Automate Release Publishing

* Create `.github/workflows/release.yml`.
* Configure a tag-based trigger.
* Run automated tests.
* Build the application.
* Package the output.
* Publish a GitHub Release.
* Upload the generated artifact.

### 🖆 Exercise: Configure Release Notes

* Create `.github/release.yml`.
* Define release categories.
* Apply labels to pull requests.
* Generate release notes.
* Review the generated output.

# 🖆 Interview Questions

**What is a GitHub Release?**

A GitHub Release is a GitHub feature for publishing a specific version of a repository using a Git tag, release notes, and downloadable assets.

**What is the difference between a Git tag and a GitHub Release?**

A Git tag identifies a commit in Git history. A GitHub Release adds publishing metadata, release notes, and downloadable assets around a tag.

**What is Semantic Versioning?**

Semantic Versioning is a versioning convention using `MAJOR.MINOR.PATCH` to communicate breaking changes, new functionality, and bug fixes.

**What is an annotated Git tag?**

An annotated tag is a Git tag object that stores metadata such as the tagger, date, and message.

**What is a pre-release?**

A pre-release is a version published for testing or early evaluation before a stable release.

**What is a draft release?**

A draft release is an unpublished release that can be prepared and reviewed before publication.

**What are release assets?**

Release assets are downloadable files attached to a GitHub Release, such as binaries, installers, archives, and checksums.

**Can a Git tag exist without a GitHub Release?**

Yes. Git tags are Git references and do not require a corresponding GitHub Release.

**How can GitHub Actions automate releases?**

A workflow can trigger on version tags, run tests, build the application, package artifacts, and create a GitHub Release.

**What is the purpose of `.github/release.yml`?**

It configures categories and labels used by GitHub's automatically generated release notes.

**How can you create a GitHub Release using GitHub CLI?**

Use:

```bash
gh release create v1.0.0
```

**How can you upload assets to an existing release?**

Use:

```bash
gh release upload v1.0.0 application.zip
```

**What is the difference between a stable release and a pre-release?**

A stable release is intended for normal use. A pre-release is intended for testing or early evaluation.

**Why should published release tags not be moved casually?**

Consumers and automation may depend on the original tag's commit. Moving it can make the same version refer to different code.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [GitHub Releases Documentation](https://docs.github.com/en/repositories/releasing-projects-on-github)
* [About Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
* [Creating and Editing Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
* [Automatically Generated Release Notes](https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes)
* [Managing Release Assets](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
* [Git Tags Documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
* [Semantic Versioning](https://semver.org/)
* [GitHub CLI Release Manual](https://cli.github.com/manual/gh_release)
* [GitHub Actions Documentation](https://docs.github.com/en/actions)
