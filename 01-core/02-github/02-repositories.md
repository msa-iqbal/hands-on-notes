# GitHub Repositories

A GitHub repository is the central place where a software project is stored, versioned, and shared.

It contains project files, directories, Git history, branches, and collaboration-related information.

GitHub adds repository management features such as:

* File browsing and editing.
* Commit history.
* Branch management.
* Access control.
* Pull requests.
* Issues.
* Releases.
* Repository insights.
* Automated workflows.

Understanding repositories is essential for working with GitHub effectively.

# 🗁 What Is a GitHub Repository?

A GitHub repository is a hosted Git repository that stores a project's files and version history.

It can contain:

* Application source code.
* Documentation.
* Configuration files.
* Images and assets.
* Tests.
* Build scripts.
* Dependency definitions.
* GitHub workflow configurations.

Example repository URL:

```text
https://github.com/username/my-project
```

A repository has two closely related parts:

**Working project content:** The files and directories that make up the project.

**Git version history:** Commits, branches, tags, and other Git metadata that record the project's development.

GitHub provides a web interface for managing and exploring these resources.

# 🗁 Repository Structure

A typical software repository might look like this:

```text
my-project/
├── .github/
│   ├── workflows/
│   │   └── ci.yml
│   └── ISSUE_TEMPLATE/
├── public/
├── src/
│   ├── components/
│   ├── pages/
│   └── utils/
├── tests/
├── .gitignore
├── LICENSE
├── package.json
├── README.md
└── CONTRIBUTING.md
```

### 🖆 Important Repository Files

| File or Directory | Purpose |
| --- | --- |
| `.github/` | GitHub-specific configuration and workflows |
| `README.md` | Project overview and documentation |
| `.gitignore` | Specifies intentionally untracked files |
| `LICENSE` | Defines permissions and restrictions for using the project |
| `CONTRIBUTING.md` | Explains contribution guidelines |
| `package.json` | JavaScript project metadata and dependencies |
| `tests/` | Automated tests |
| `src/` | Application source code |

The structure depends on the programming language, framework, and project requirements.

# 🗁 Creating a Repository

You can create a GitHub repository through the website or GitHub CLI.

### 🖆 Create a Repository Through the Website

Open:

<https://github.com/new>

Configure the repository:

* **Owner:** Select your personal account or an organization.
* **Repository name:** Enter a unique name under the selected owner.
* **Description:** Add a short explanation of the project.
* **Visibility:** Choose public or private.
* **Add README:** Optionally initialize the repository with a README.
* **Add .gitignore:** Optionally select a template.
* **Choose a license:** Optionally select an appropriate license.

Click **Create repository**.

### 🖆 Create a Repository Using GitHub CLI

Authenticate first:

```bash
gh auth login
```

Create a repository:

```bash
gh repo create my-project
```

Create a public repository:

```bash
gh repo create my-project --public
```

Create a private repository:

```bash
gh repo create my-project --private
```

Create a repository and clone it locally:

```bash
gh repo create my-project --public --clone
```

You can also create a repository from an existing local directory:

```bash
gh repo create my-project --source=. --public --push
```

This uses the current directory as the source and pushes its contents to the newly created repository.

# 🗁 Repository Initialization

A GitHub repository can be initialized with several optional files.

### 🖆 Initialize with README

A README introduces the project and explains how to use it.

GitHub automatically displays a repository's root-level README on its main page.

### 🖆 Initialize with .gitignore

A `.gitignore` file tells Git which untracked files and directories to ignore.

For example:

```gitignore
node_modules/
.env
dist/
build/
*.log
```

### 🖆 Initialize with a License

A license defines the terms under which others can use, modify, and distribute your project.

Common open-source licenses include:

* MIT License.
* Apache License 2.0.
* GNU General Public License (GPL).

Choose a license based on your project's requirements.

**Important:** Creating a repository without a license does not automatically grant users open-source permissions. Copyright generally remains with the rights holder.

### 🖆 Empty Repository vs Initialized Repository

| Empty Repository | Initialized Repository |
| --- | --- |
| Starts without an initial commit | Starts with an initial commit |
| Suitable for pushing an existing local repository | Suitable for starting a new project on GitHub |
| No README unless added later | May contain a README |
| No initial branch content until a commit exists | Has an initial branch and commit |

When connecting an existing local repository, starting with an empty GitHub repository can simplify the initial push.

# 🗁 Cloning a Repository

Cloning downloads a remote repository to your local machine, including its Git history.

### 🖆 Clone Using HTTPS

```bash
git clone https://github.com/username/my-project.git
```

### 🖆 Clone Using SSH

```bash
git clone git@github.com:username/my-project.git
```

SSH requires an appropriately configured SSH key.

### 🖆 Clone into a Custom Directory

```bash
git clone https://github.com/username/my-project.git my-local-project
```

### 🖆 Clone Only a Specific Branch

```bash
git clone --branch development https://github.com/username/my-project.git
```

### 🖆 Shallow Clone

A shallow clone downloads limited commit history.

```bash
git clone --depth 1 https://github.com/username/my-project.git
```

This can be useful when you need the latest project state without the complete history.

### 🖆 Verify the Clone

Enter the directory:

```bash
cd my-project
```

Check the remote:

```bash
git remote -v
```

Check the working tree:

```bash
git status
```

# 🗁 Connecting a Local Repository to GitHub

You may already have a local Git repository that you want to publish on GitHub.

### 🖆 Create a Local Repository

```bash
mkdir my-project

cd my-project

git init
```

Create a project file:

```bash
echo "# My Project" > README.md
```

Stage and commit the file:

```bash
git add README.md

git commit -m "Initial commit"
```

### 🖆 Create an Empty GitHub Repository

Create a new repository on GitHub without initializing it with a README, license, or `.gitignore`.

### 🖆 Add the Remote

Using HTTPS:

```bash
git remote add origin https://github.com/username/my-project.git
```

Using SSH:

```bash
git remote add origin git@github.com:username/my-project.git
```

### 🖆 Verify the Remote

```bash
git remote -v
```

Expected output:

```text
origin  https://github.com/username/my-project.git (fetch)
origin  https://github.com/username/my-project.git (push)
```

### 🖆 Push the Local Branch

Rename the current branch to `main` if needed:

```bash
git branch -M main
```

Push it to GitHub:

```bash
git push -u origin main
```

The `-u` option establishes the upstream tracking relationship between the local and remote branches.

Future pushes can usually use:

```bash
git push
```

# 🗁 Repository URLs

GitHub supports different URL formats for accessing repositories.

### 🖆 HTTPS URL

```text
https://github.com/username/repository.git
```

HTTPS is commonly used for cloning and remote operations.

For authenticated Git operations, GitHub supports credential-based authentication, such as personal access tokens through supported credential managers. Your GitHub account password is not used for Git HTTPS authentication.

### 🖆 SSH URL

```text
git@github.com:username/repository.git
```

SSH uses cryptographic keys for authentication.

It is useful for developers who regularly interact with remote repositories.

### 🖆 Web URL

```text
https://github.com/username/repository
```

This opens the repository in a browser.

### 🖆 Check Remote URLs

```bash
git remote -v
```

### 🖆 Change the Remote URL

```bash
git remote set-url origin https://github.com/username/new-repository.git
```

Or use SSH:

```bash
git remote set-url origin git@github.com:username/new-repository.git
```

# 🗁 Repository Visibility

GitHub provides public and private repository visibility.

### 🖆 Public Repository

Public repositories are visible to everyone.

Anyone can generally:

* View repository files.
* Read documentation.
* Clone the repository.
* Fork the repository.

Write access still requires appropriate permissions.

### 🖆 Private Repository

Private repositories are restricted to authorized users.

Common use cases include:

* Private application development.
* Internal business software.
* Personal projects.
* Proprietary source code.

### 🖆 Change Repository Visibility

Open the repository:

```text
Repository → Settings → General
```

Locate the repository visibility section and use the available controls to change visibility.

Changing visibility can affect access, integrations, forks, and how repository content is exposed.

Review the consequences before changing it.

# 🗁 Repository Settings

Repository settings control how a repository behaves and who can interact with it.

Open:

```text
Repository → Settings
```

Common settings include:

| Setting Area | Purpose |
| --- | --- |
| General | Repository name, description, features, visibility |
| Collaborators and teams | Manage repository access |
| Branches | Configure branch protection and rulesets |
| Actions | Configure workflow permissions |
| Webhooks | Integrate external services |
| Deploy keys | Configure SSH access for automation or deployment |
| Secrets and variables | Store values for GitHub Actions workflows |
| Environments | Configure deployment targets and protection rules |
| Pages | Configure GitHub Pages |
| Security | Configure repository security features |

The exact available settings depend on repository ownership, permissions, and GitHub plan.

### 🖆 Rename a Repository

Open:

```text
Repository → Settings → General → Repository name
```

Enter the new name and save the changes.

GitHub generally provides redirects from the old repository URL, but you should still update local remotes and external integrations.

Update your local remote:

```bash
git remote set-url origin https://github.com/username/new-name.git
```

# 🗁 Repository Access and Permissions

Repository permissions determine what users can do.

GitHub provides predefined repository roles.

| Role | General Capabilities |
| --- | --- |
| Read | View and clone repository content |
| Triage | Manage issues and pull requests without write access |
| Write | Push changes and collaborate |
| Maintain | Manage repository operations without full administrative control |
| Admin | Manage repository settings and access |

The precise capabilities vary by resource and configuration.

### 🖆 Add a Collaborator

For a personal repository:

* Open the repository.
* Navigate to Settings.
* Open Collaborators or the access-management section.
* Invite the GitHub user.
* Assign the appropriate access.

The invited user must accept the invitation when required.

### 🖆 Organization Repository Access

Organizations can grant access through:

* Individual members.
* Teams.
* Repository roles.
* Organization-level policies.

Using teams is generally easier to manage than assigning permissions individually to many users.

### 🖆 Principle of Least Privilege

Give each user only the permissions required for their responsibilities.

For example:

* Developers may need Write access.
* Reviewers may need Read access.
* Project coordinators may need Triage access.
* Repository administrators need Admin access.

# 🗁 Managing Repository Files

GitHub allows you to manage files directly through the web interface.

### 🖆 Create a File

Open a repository and select:

```text
Add file → Create new file
```

Enter the filename and its content, then commit the change.

### 🖆 Upload Files

Select:

```text
Add file → Upload files
```

Choose files from your computer and commit the changes.

### 🖆 Edit a File

Open the file and select the edit option.

Make your changes and commit them.

### 🖆 Delete a File

Open the file and use the delete option.

Commit the deletion.

### 🖆 Create Directories

Git does not track empty directories.

To create a directory through GitHub, create a file using a path:

```text
docs/getting-started.md
```

GitHub creates the directory structure when the file is committed.

### 🖆 Important Considerations

Web-based file editing is useful for small changes.

For larger projects, local development is generally more practical because it provides:

* A complete development environment.
* Testing tools.
* IDE support.
* Build tools.
* Better control over multiple files.
* Git command-line workflows.

# 🗁 README Files

A `README.md` file provides information about a repository.

GitHub automatically renders the root-level README on the repository's main page.

### 🖆 Common README Sections

```markdown
# Project Name

Short project description.

# 🗁 Overview

Explain what the project does.

# 🗁 Features

- Feature One
- Feature Two
- Feature Three

# 🗁 Technologies

- JavaScript
- React
- Node.js

# 🗁 Installation

Explain how to install dependencies.

# 🗁 Usage

Explain how to run the project.

# 🗁 Project Structure

Describe the main directories.

# 🗁 Contributing

Explain how others can contribute.

# 🗁 License

Specify the project license.
```

### 🖆 Why README Matters

A good README helps developers:

* Understand the project's purpose.
* Install dependencies.
* Run the application.
* Understand the architecture.
* Contribute to development.
* Find relevant documentation.

For most software projects, a README should be one of the first files created.

# 🗁 The `.gitignore` File

The `.gitignore` file specifies intentionally untracked files that Git should ignore.

Example:

```gitignore
# Dependencies
node_modules/

# Environment files
.env
.env.local

# Build output
dist/
build/

# Logs
*.log

# Operating system files
.DS_Store
Thumbs.db

# Editor settings
.vscode/
.idea/
```

### 🖆 Why Use `.gitignore`?

It prevents unnecessary files from being added to version control.

Common examples include:

* Dependencies.
* Build artifacts.
* Temporary files.
* Local environment configuration.
* Editor-specific files.
* Operating system metadata.

### 🖆 💡 IMPORTANT: `.gitignore` Does Not Untrack Existing Files

If a file is already tracked by Git, adding it to `.gitignore` does not automatically remove it from version control.

To stop tracking a file while keeping it locally:

```bash
git rm --cached .env
```

Then commit the change:

```bash
git add .gitignore

git commit -m "Stop tracking environment file"
```

If a directory is already tracked:

```bash
git rm -r --cached node_modules/
```

Never rely on `.gitignore` as a way to remove secrets that have already been committed. A leaked credential should be revoked or rotated, and the repository history may also need remediation.

# 🗁 Repository Branches

Branches allow developers to work on different features or fixes independently.

A repository can contain multiple branches.

Example:

```text
main
development
feature/login
feature/dashboard
bugfix/payment-error
```

### 🖆 View Branches

List local branches:

```bash
git branch
```

List remote-tracking branches:

```bash
git branch -r
```

List all branches:

```bash
git branch -a
```

### 🖆 Create a Branch

```bash
git switch -c feature/login
```

### 🖆 Push a Branch to GitHub

```bash
git push -u origin feature/login
```

### 🖆 Delete a Local Branch

```bash
git branch -d feature/login
```

### 🖆 Delete a Remote Branch

```bash
git push origin --delete feature/login
```

Branches are covered in detail in the Git section's branching and merging chapters.

# 🗁 Default Branch

The default branch is the primary branch GitHub selects when users open a repository or create certain repository-based workflows.

It is commonly named:

```text
main
```

Other names are possible, such as:

```text
master
development
```

### 🖆 Change the Default Branch

Open:

```text
Repository → Settings → General → Default branch
```

Select the desired branch and save the change.

Before changing the default branch, consider:

* Existing pull requests.
* Automation workflows.
* Deployment configurations.
* External integrations.
* Local branch tracking.
* Documentation references.

Changing the default branch on GitHub does not automatically rename every developer's local branch.

# 🗁 Branch Protection and Rulesets

Branch protection and repository rules help prevent accidental or unauthorized changes to important branches.

They can enforce policies such as:

* Requiring pull requests before merging.
* Requiring approvals.
* Requiring status checks to pass.
* Requiring conversations to be resolved.
* Restricting force pushes.
* Restricting branch deletion.
* Requiring signed commits in supported configurations.

### 🖆 Example: Protect the Main Branch

A common team policy is:

```text
main
 |
 ├── Direct pushes restricted
 ├── Pull request required
 ├── At least one approval required
 ├── Required CI checks must pass
 └── Conversations must be resolved
```

GitHub provides branch protection rules and repository rulesets.

Rulesets provide a more flexible way to manage repository rules and can target branches, tags, and other supported repository resources.

Configure them under the repository's Settings area.

# 🗁 Repository Topics

Topics are labels that describe what a repository is about.

Examples:

```text
javascript
react
nodejs
typescript
web-development
api
machine-learning
```

Topics help users discover repositories through GitHub search and topic pages.

### 🖆 Add Topics

Open the repository's main page.

Locate the About section and edit the topics.

Use relevant and specific topics rather than adding unrelated keywords.

# 🗁 Repository Templates

A repository template provides a reusable starting point for new repositories.

It is useful when multiple projects share a similar structure.

For example, a web application template might contain:

```text
web-template/
├── .github/
│   └── workflows/
├── src/
├── tests/
├── .gitignore
├── LICENSE
├── README.md
└── package.json
```

### 🖆 Create a Template Repository

* Open the repository.
* Navigate to Settings.
* Locate the template repository option.
* Enable it.

Other users can then use the repository as a template when creating a new repository.

### 🖆 Template vs Fork

| Template | Fork |
| --- | --- |
| Creates a new repository from template contents | Creates a fork relationship with the source repository |
| Intended as a reusable starting point | Intended for contributing or maintaining a related copy |
| Does not establish the same fork network relationship | Retains the relationship with the upstream repository |
| Useful for project scaffolding | Useful for collaborative development |

# 🗁 Forking a Repository

A fork creates a copy of a repository under another account or organization.

Forking is commonly used in open-source contribution workflows.

### 🖆 Fork Workflow

```text
Original Repository
        |
        v
     Fork
        |
        v
Your GitHub Account
        |
        v
Clone Your Fork
        |
        v
Create Feature Branch
        |
        v
Make Changes
        |
        v
Push to Your Fork
        |
        v
Create Pull Request
        |
        v
Original Repository
```

### 🖆 Fork Through GitHub

* Open the original repository.
* Click Fork.
* Choose the destination owner.
* Configure the repository name and available options.
* Create the fork.

### 🖆 Clone Your Fork

```bash
git clone https://github.com/your-username/project.git
```

### 🖆 Add the Original Repository as Upstream

```bash
git remote add upstream https://github.com/original-owner/project.git
```

Check remotes:

```bash
git remote -v
```

Example output:

```text
origin    https://github.com/your-username/project.git (fetch)
origin    https://github.com/your-username/project.git (push)

upstream  https://github.com/original-owner/project.git (fetch)
upstream  https://github.com/original-owner/project.git (push)
```

### 🖆 Fetch Upstream Changes

```bash
git fetch upstream
```

Update your local main branch:

```bash
git switch main

git merge upstream/main
```

Push the updated branch to your fork:

```bash
git push origin main
```

This keeps your fork synchronized with the original repository.

# 🗁 Repository Insights

GitHub provides repository insights that help maintainers understand project activity.

Depending on the repository and available features, insights can include:

* Contributors.
* Commit activity.
* Code frequency.
* Dependency information.
* Traffic.
* Network information.
* Community activity.

### 🖆 Contributors

The contributors view shows people who have contributed commits to the repository.

### 🖆 Commit Activity

Commit activity helps visualize development over time.

### 🖆 Traffic

Traffic insights can provide information about repository views and clones over a limited reporting period.

### 🖆 Network

The network graph can help visualize branches and repository relationships.

Insights are useful for understanding project health and development activity.

# 🗁 Releases and Tags

Git tags identify specific points in Git history.

GitHub releases provide a way to publish and describe project versions.

Example version tags:

```text
v1.0.0
v1.1.0
v2.0.0
```

### 🖆 Create a Tag Locally

```bash
git tag v1.0.0
```

Push the tag:

```bash
git push origin v1.0.0
```

### 🖆 Create a Release on GitHub

Open:

```text
Repository → Releases → Draft a new release
```

Then:

* Select or create a tag.
* Add a release title.
* Write release notes.
* Attach downloadable assets if needed.
* Publish the release.

### 🖆 Tag vs Release

| Git Tag | GitHub Release |
| --- | --- |
| Git reference to a commit | Published release information on GitHub |
| Created and managed through Git | Created and managed through GitHub |
| Can exist without GitHub | Associated with a repository and tag |
| Identifies a version or commit | Describes and distributes a version |

# 🗁 Archiving and Deleting Repositories

GitHub provides options to archive or delete repositories.

These operations have different consequences.

### 🖆 Archive a Repository

Archiving makes a repository read-only.

It is useful when:

* A project is no longer actively maintained.
* Development has stopped.
* A project should remain available for reference.
* The repository should be preserved without further changes.

Archived repositories remain accessible according to their visibility and permissions.

### 🖆 Delete a Repository

Deleting a repository removes it from GitHub and can affect associated resources and integrations.

Before deletion:

* Confirm that the repository is no longer needed.
* Back up important data.
* Check dependencies and external references.
* Review the consequences for forks, automation, and collaborators.

### 🖆 Archive vs Delete

| Archive | Delete |
| --- | --- |
| Preserves the repository in read-only form | Removes the repository from GitHub |
| Useful for inactive projects | Useful when a repository should be removed |
| Repository remains accessible | Repository becomes unavailable through its normal location |
| Can be unarchived by an authorized user | Recovery may be limited and time-dependent |

Treat deletion as a destructive operation.

# 🗁 Repository Transfer

GitHub allows eligible repositories to be transferred to another owner, such as another personal account or organization.

A transfer can be useful when:

* A personal project becomes an organization project.
* Ownership changes.
* A repository moves between organizations.

### 🖆 Before Transferring

Review:

* The destination owner's permissions.
* Repository name availability.
* Collaborator access.
* External integrations.
* Actions workflows and secrets.
* Package and Pages dependencies.
* Existing references to the repository URL.

After a transfer, update local remote URLs if necessary:

```bash
git remote set-url origin https://github.com/new-owner/repository.git
```

# 🗁 GitHub Repository Workflow

A practical repository workflow combines local Git operations with GitHub's collaboration features.

```text
Create Repository on GitHub
          |
          v
Clone Repository Locally
          |
          v
Create Feature Branch
          |
          v
Develop and Test
          |
          v
Commit Changes
          |
          v
Push Branch
          |
          v
Open Pull Request
          |
          v
Code Review and CI Checks
          |
          v
Merge into Main
          |
          v
Delete Feature Branch
          |
          v
Update Local Main
```

### 🖆 Example Commands

Clone:

```bash
git clone https://github.com/username/project.git

cd project
```

Create a branch:

```bash
git switch -c feature/new-feature
```

Make changes and commit:

```bash
git add .

git commit -m "Add new feature"
```

Push:

```bash
git push -u origin feature/new-feature
```

After the pull request is merged:

```bash
git switch main

git pull origin main
```

Delete the local feature branch:

```bash
git branch -d feature/new-feature
```

# 🖆 Common Problems and Troubleshooting

### 🖆 Repository Already Exists

GitHub repository names must be unique within the selected owner.

Choose another repository name or use the existing repository if it is yours and appropriate for the project.

### 🖆 Remote Origin Already Exists

Error:

```text
error: remote origin already exists.
```

Check configured remotes:

```bash
git remote -v
```

Update the existing remote:

```bash
git remote set-url origin https://github.com/username/project.git
```

Alternatively, remove and recreate it:

```bash
git remote remove origin

git remote add origin https://github.com/username/project.git
```

### 🖆 Failed to Push Because Remote Contains Work

Error:

```text
! [rejected] main -> main (fetch first)
```

The remote branch contains commits that are missing locally.

Fetch and integrate the remote changes:

```bash
git pull --rebase origin main
```

Resolve conflicts if necessary, then push:

```bash
git push origin main
```

### 🖆 Repository Not Found

Check:

* Repository URL.
* Repository owner.
* Repository name.
* Repository visibility.
* Your account permissions.
* Authentication configuration.

Verify the remote:

```bash
git remote -v
```

### 🖆 Permission Denied

Possible causes:

* Your account does not have the required repository role.
* SSH authentication is not configured.
* The wrong GitHub account is being used.
* A token lacks the required permissions.
* Organization access policies restrict your access.

Check your repository access and authentication method.

### 🖆 Accidentally Committed a Sensitive File

If a secret was committed:

* Revoke or rotate the exposed credential immediately.
* Remove the secret from the current project files.
* Prevent it from being committed again.
* Review whether repository history needs cleanup.
* Coordinate with collaborators if history rewriting is required.

Removing a file in a later commit does not remove it from earlier commits.

### 🖆 Branch Not Visible on GitHub

Check whether the branch exists locally:

```bash
git branch
```

Push it:

```bash
git push -u origin branch-name
```

Then refresh the repository's branch list.

# 🖆 Best Practices

* Use meaningful repository names.
* Add a clear description.
* Include a useful README.
* Select an appropriate license.
* Maintain a suitable `.gitignore`.
* Use public visibility only when intended.
* Keep private project access restricted.
* Organize files consistently.
* Use feature branches for development.
* Protect important branches.
* Require reviews for critical changes.
* Keep repository topics relevant.
* Use templates for repeated project structures.
* Review collaborators and permissions regularly.
* Archive inactive projects instead of deleting them when preservation is useful.
* Keep releases and tags consistent.
* Never commit credentials or secrets.
* Keep local remotes and repository references up to date.

# 🖆 Practical Exercises

### 🖆 Exercise: Create a Repository

* [ ] Create a repository named `repository-practice`.
* [ ] Add a description.
* [ ] Choose public or private visibility.
* [ ] Initialize it with a README.
* [ ] Add a `.gitignore`.
* [ ] Add a license if appropriate.
* [ ] Explore the repository settings.

### 🖆 Exercise: Connect a Local Repository

* [ ] Create a local project directory.
* [ ] Initialize Git.
* [ ] Create a README.
* [ ] Make an initial commit.
* [ ] Create an empty GitHub repository.
* [ ] Add the GitHub remote.
* [ ] Push the `main` branch.
* [ ] Verify the files on GitHub.

### 🖆 Exercise: Repository Management

* [ ] Add a project description.
* [ ] Add relevant repository topics.
* [ ] Create a feature branch.
* [ ] Push the branch to GitHub.
* [ ] Change the default branch in a test repository.
* [ ] Explore repository insights.
* [ ] Create a Git tag.
* [ ] Publish a test release.

### 🖆 Exercise: Fork and Synchronize

* [ ] Find a public repository that accepts contributions.
* [ ] Fork the repository.
* [ ] Clone your fork.
* [ ] Add the original repository as `upstream`.
* [ ] Fetch upstream changes.
* [ ] Create a feature branch.
* [ ] Make a small documentation change.
* [ ] Push the branch to your fork.

### 🖆 Exercise: Repository Security

* [ ] Review repository collaborators.
* [ ] Inspect repository visibility.
* [ ] Review branch protection or rulesets.
* [ ] Inspect Actions permissions.
* [ ] Explore repository secrets and variables.
* [ ] Verify that sensitive files are ignored.

# 🖆 Interview Questions

**What is a GitHub repository?**

A GitHub repository is a hosted Git repository that contains project files and version history.

**What is the difference between a local repository and a remote repository?**

A local repository exists on a developer's machine, while a remote repository is hosted on a remote Git server such as GitHub.

**How do you create a GitHub repository?**

You can create one through the GitHub website or GitHub CLI using `gh repo create`.

**What is the purpose of a README file?**

A README introduces the project and provides information about installation, usage, features, and contribution guidelines.

**What is the purpose of .gitignore?**

It specifies untracked files and directories that Git should ignore.

**Does .gitignore remove files that are already tracked?**

No. Existing tracked files remain tracked until they are explicitly removed from the index.

**What is the difference between cloning and forking?**

Cloning downloads a repository to a local machine. Forking creates a copy of a repository under another GitHub account or organization.

**What is the difference between origin and upstream?**

`origin` commonly refers to your primary remote, often your own fork. `upstream` commonly refers to the original repository from which your fork was created.

**What is a default branch?**

The default branch is the primary branch GitHub uses when opening a repository and as the default target for certain operations.

**What is branch protection?**

Branch protection enforces rules that help prevent unauthorized or unsafe changes to important branches.

**What is the difference between a Git tag and a GitHub release?**

A Git tag identifies a specific commit. A GitHub release provides published version information associated with a tag.

**What is a repository template?**

A repository template provides a reusable starting point for creating new repositories with a predefined structure.

**What is the difference between archiving and deleting a repository?**

Archiving preserves a repository in read-only form, while deleting removes it from GitHub.

**How do you change a repository's remote URL?**

Use:

```bash
git remote set-url origin <new-url>
```

**How do you push a local branch to GitHub?**

Use:

```bash
git push -u origin branch-name
```

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [GitHub Repositories Documentation](https://docs.github.com/en/repositories)
* [Creating a New Repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
* [Cloning a Repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)
* [Managing Repository Settings](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features)
* [Repository Roles](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/repository-roles-for-an-organization)
* [Managing Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches)
* [About Rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
* [Forking a Repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
* [Managing Releases](https://docs.github.com/en/repositories/releasing-projects-on-github)
* [GitHub CLI Manual](https://cli.github.com/manual/)
