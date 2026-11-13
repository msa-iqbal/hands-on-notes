# GitHub CLI (gh)

GitHub CLI is a command-line tool that allows developers to interact with GitHub directly from a terminal.

Instead of switching between a terminal and the GitHub website, you can manage repositories, issues, pull requests, releases, and GitHub Actions from the command line.

GitHub CLI is commonly referred to as `gh`.

Example:

```bash
gh repo create my-project --public
```

This command creates a new public GitHub repository.

GitHub CLI complements Git. Git manages source code and version history, while GitHub CLI interacts with GitHub's platform features.

# 🗁 What Is GitHub CLI?

**GitHub CLI (`gh`)** is the official command-line interface for GitHub.

It provides commands for interacting with GitHub resources, including:

* Repositories
* Issues
* Pull requests
* GitHub Actions
* Releases
* Gists
* Notifications
* Projects
* Discussions

GitHub CLI communicates with GitHub through its APIs and other supported mechanisms.

### 🖆 Basic command structure

```bash
gh <command> <subcommand> [flags]
```

Example:

```bash
gh repo list
```

Explanation:

| Component | Description |
| --- | --- |
| `gh` | GitHub CLI executable |
| `repo` | Repository command group |
| `list` | Lists repositories |

Another example:

```bash
gh pr create --title "Add login feature" --body "Implements user login."
```

This creates a pull request using the supplied title and description.

# 🗁 Why Use GitHub CLI?

GitHub CLI makes GitHub workflows easier to automate and manage.

**Main advantages:**

* Manage GitHub repositories without opening a browser.
* Create and review pull requests from the terminal.
* Manage issues directly from development environments.
* Monitor GitHub Actions workflows.
* Create and manage releases.
* Automate repetitive GitHub operations.
* Integrate GitHub workflows into shell scripts.
* Work efficiently with GitHub from remote development environments.

# 🗁 Git vs GitHub CLI

Git and GitHub CLI serve different purposes.

| Feature | Git | GitHub CLI |
| --- | --- | --- |
| Purpose | Version control | GitHub platform management |
| Repository history | Yes | No, delegates to Git where appropriate |
| Commits | Yes | Not its primary responsibility |
| Branch management | Yes | Can interact with pull-request branches |
| Merge operations | Yes | Can merge pull requests |
| Issues | No native GitHub issue management | Yes |
| Pull requests | No native GitHub pull-request management | Yes |
| GitHub Actions | No | Yes |
| Releases | Git tags | GitHub releases |
| Authentication | SSH or HTTPS credentials | GitHub authentication |

### 🖆 Git example

```bash
git status
git add .
git commit -m "Add authentication"
git push origin main
```

### 🖆 GitHub CLI example

```bash
gh issue list
gh pr list
gh run list
gh release list
```

**Remember:**

* Use Git for version control.
* Use GitHub CLI for GitHub platform operations.

Both tools work together in a typical development workflow.

# 🗁 Installation

GitHub CLI supports Linux, macOS, and Windows.

Official installation documentation:

<https://cli.github.com/>

### 🖆 Linux (Ubuntu / Debian)

Install using the official GitHub CLI package repository:

```bash
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) &&
sudo mkdir -p -m 755 /etc/apt/keyrings &&
out=$(mktemp) &&
wget -nv -O "$out" https://cli.github.com/packages/githubcli-archive-keyring.gpg &&
sudo install -D -o root -g root -m 644 "$out" /etc/apt/keyrings/githubcli-archive-keyring.gpg &&
sudo mkdir -p /etc/apt/sources.list.d &&
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" |
sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null &&
sudo apt update &&
sudo apt install gh -y
```

### 🖆 Fedora

```bash
sudo dnf install gh
```

### 🖆 Arch Linux

```bash
sudo pacman -S github-cli
```

### 🖆 macOS

Using Homebrew:

```bash
brew install gh
```

### 🖆 Windows

Using Windows Package Manager:

```powershell
winget install --id GitHub.cli
```

Alternatively, install through Scoop:

```powershell
scoop install gh
```

Use the official installation documentation for additional package managers and platform-specific instructions.

# 🗁 Verify Installation

After installation, check the installed version:

```bash
gh --version
```

Example:

```text
gh version 2.x.x
```

Display available commands:

```bash
gh help
```

Get help for a specific command:

```bash
gh repo --help
```

You can also use:

```bash
gh repo create --help
```

The help system explains available options, flags, and usage examples.

# 🗁 Authenticate with GitHub

Before using commands that access private or account-specific GitHub resources, authenticate with GitHub.

Start the authentication process:

```bash
gh auth login
```

You will be prompted to select:

* GitHub.com or a GitHub Enterprise Server host.
* HTTPS or SSH for Git operations.
* An authentication method.

A common configuration is:

```text
What account do you want to log into?
> GitHub.com

What is your preferred protocol for Git operations?
> HTTPS

How would you like to authenticate GitHub CLI?
> Login with a web browser
```

GitHub CLI displays a one-time code and asks you to open a browser.

Follow the instructions to authorize the CLI.

### 🖆 Verify authentication

```bash
gh auth status
```

This displays the authenticated GitHub account and host.

### 🖆 Log out

```bash
gh auth logout
```

This removes the stored authentication for the selected account and host.

### 🖆 Switch accounts

If you have authenticated multiple accounts:

```bash
gh auth switch
```

You can select which account should be active for the current GitHub host.

# 🗁 Authentication and Configuration

GitHub CLI stores authentication and configuration information locally.

View the current configuration directory:

```bash
gh config get editor
```

Set your preferred editor:

```bash
gh config set editor "code --wait"
```

Set the preferred Git protocol:

```bash
gh config set git_protocol ssh
```

Or use HTTPS:

```bash
gh config set git_protocol https
```

List available configuration options:

```bash
gh config list
```

The exact configuration options depend on the installed GitHub CLI version.

### 🖆 Environment variables

GitHub CLI supports environment variables for authentication and configuration.

Common examples:

| Variable | Purpose |
| --- | --- |
| `GH_TOKEN` | Authentication token |
| `GH_HOST` | GitHub host |
| `GH_REPO` | Default repository |
| `GH_EDITOR` | Preferred editor |
| `GH_PAGER` | Preferred pager |
| `GH_PROMPT_DISABLED` | Disables interactive prompts |

Example:

```bash
export GH_TOKEN="YOUR_TOKEN"
```

Avoid placing actual tokens directly in shell scripts, source code, or committed configuration files.

For automated workflows, use GitHub Actions secrets or an appropriate secure credential store.

# 🗁 Repository Management

GitHub CLI provides the `repo` command group for repository operations.

### 🖆 List repositories

List your repositories:

```bash
gh repo list
```

List repositories belonging to an organization:

```bash
gh repo list organization-name
```

Limit the number of results:

```bash
gh repo list --limit 20
```

### 🖆 View repository details

```bash
gh repo view
```

View a specific repository:

```bash
gh repo view username/repository
```

Open the repository in a browser:

```bash
gh repo view --web
```

### 🖆 Create a repository

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

Create a repository with a description:

```bash
gh repo create my-project \
  --public \
  --description "My development project"
```

### 🖆 Clone a repository

```bash
gh repo clone username/repository
```

This clones the repository using the Git protocol configured in GitHub CLI.

### 🖆 Fork a repository

```bash
gh repo fork username/repository
```

Fork and clone it locally:

```bash
gh repo fork username/repository --clone
```

### 🖆 Delete a repository

```bash
gh repo delete username/repository
```

GitHub CLI may request confirmation before deleting the repository.

Deletion is destructive. Verify the repository name and ownership before proceeding.

# 🗁 Issues Management

GitHub Issues are used to track bugs, feature requests, tasks, and project discussions.

GitHub CLI provides the `issue` command group.

### 🖆 List issues

```bash
gh issue list
```

List issues with a specific state:

```bash
gh issue list --state open
```

```bash
gh issue list --state closed
```

Filter issues by label:

```bash
gh issue list --label bug
```

Filter by assignee:

```bash
gh issue list --assignee username
```

### 🖆 View an issue

```bash
gh issue view 12
```

Open an issue in the browser:

```bash
gh issue view 12 --web
```

### 🖆 Create an issue

```bash
gh issue create
```

Create an issue with a title and body:

```bash
gh issue create \
  --title "Fix login validation" \
  --body "The login form accepts invalid email addresses."
```

Add a label:

```bash
gh issue create \
  --title "Improve performance" \
  --body "Optimize database queries." \
  --label enhancement
```

Assign an issue:

```bash
gh issue create \
  --title "Update documentation" \
  --body "Improve installation instructions." \
  --assignee username
```

### 🖆 Edit an issue

```bash
gh issue edit 12 --title "Updated issue title"
```

Add a label:

```bash
gh issue edit 12 --add-label bug
```

Assign an issue:

```bash
gh issue edit 12 --add-assignee username
```

### 🖆 Close an issue

```bash
gh issue close 12
```

Reopen an issue:

```bash
gh issue reopen 12
```

### 🖆 Comment on an issue

```bash
gh issue comment 12 --body "I am investigating this issue."
```

# 🗁 Pull Request Management

Pull requests allow developers to propose, review, and merge changes into a repository.

GitHub CLI makes pull-request workflows available directly from the terminal.

### 🖆 List pull requests

```bash
gh pr list
```

List open pull requests:

```bash
gh pr list --state open
```

List merged pull requests:

```bash
gh pr list --state merged
```

### 🖆 View a pull request

```bash
gh pr view 25
```

Open it in a browser:

```bash
gh pr view 25 --web
```

### 🖆 Create a pull request

First, push your feature branch:

```bash
git push -u origin feature/login
```

Create a pull request:

```bash
gh pr create
```

Create one with a title and description:

```bash
gh pr create \
  --title "Add login functionality" \
  --body "Implements the user authentication workflow."
```

Specify the base branch:

```bash
gh pr create \
  --base main \
  --title "Add login functionality" \
  --body "Implements user authentication."
```

Specify the head branch:

```bash
gh pr create \
  --head feature/login \
  --base main \
  --title "Add login functionality" \
  --body "Implements user authentication."
```

### 🖆 Checkout a pull request

Fetch and check out a pull-request branch locally:

```bash
gh pr checkout 25
```

This is useful when reviewing or testing someone else's changes.

### 🖆 Review a pull request

View its details:

```bash
gh pr view 25
```

Check out the branch:

```bash
gh pr checkout 25
```

Review its changes:

```bash
gh pr diff 25
```

Submit a review:

```bash
gh pr review 25 --approve
```

Request changes:

```bash
gh pr review 25 --request-changes --body "Please handle the validation error."
```

Add a review comment:

```bash
gh pr review 25 --comment --body "The implementation looks good."
```

### 🖆 Merge a pull request

Merge using a merge commit:

```bash
gh pr merge 25 --merge
```

Merge using squash:

```bash
gh pr merge 25 --squash
```

Merge using rebase:

```bash
gh pr merge 25 --rebase
```

Delete the head branch after merging:

```bash
gh pr merge 25 --squash --delete-branch
```

The merge method must be allowed by the repository's settings.

### 🖆 Close a pull request

```bash
gh pr close 25
```

Reopen it:

```bash
gh pr reopen 25
```

# 🗁 GitHub Actions Management

GitHub Actions automates software development workflows.

GitHub CLI provides the `run` and `workflow` command groups.

### 🖆 List workflow runs

```bash
gh run list
```

Limit the number of results:

```bash
gh run list --limit 10
```

### 🖆 View a workflow run

```bash
gh run view RUN_ID
```

View logs:

```bash
gh run view RUN_ID --log
```

### 🖆 Watch a workflow run

```bash
gh run watch RUN_ID
```

This waits for the workflow to finish and displays its progress.

### 🖆 Rerun a workflow

```bash
gh run rerun RUN_ID
```

Rerun failed jobs:

```bash
gh run rerun RUN_ID --failed
```

### 🖆 Cancel a workflow

```bash
gh run cancel RUN_ID
```

### 🖆 List workflows

```bash
gh workflow list
```

### 🖆 View workflow details

```bash
gh workflow view
```

View a specific workflow:

```bash
gh workflow view ci.yml
```

### 🖆 Run a workflow manually

```bash
gh workflow run ci.yml
```

Pass an input:

```bash
gh workflow run deploy.yml -f environment=staging
```

This requires a workflow configured with a compatible `workflow_dispatch` trigger and input.

### 🖆 Enable and disable workflows

Disable:

```bash
gh workflow disable ci.yml
```

Enable:

```bash
gh workflow enable ci.yml
```

# 🗁 Release Management

GitHub Releases provide a way to publish versioned software releases.

GitHub CLI supports creating, listing, viewing, editing, and deleting releases.

### 🖆 List releases

```bash
gh release list
```

### 🖆 View a release

```bash
gh release view
```

View a specific version:

```bash
gh release view v1.0.0
```

### 🖆 Create a release

```bash
gh release create v1.0.0
```

Create a release with a title and notes:

```bash
gh release create v1.0.0 \
  --title "Version 1.0.0" \
  --notes "First stable release."
```

Create a draft release:

```bash
gh release create v1.0.0 --draft
```

Create a prerelease:

```bash
gh release create v1.1.0-beta.1 --prerelease
```

### 🖆 Generate release notes automatically

```bash
gh release create v1.0.0 --generate-notes
```

GitHub generates release notes based on the repository's release history and configuration.

### 🖆 Upload release assets

```bash
gh release create v1.0.0 \
  dist/application.zip \
  dist/checksum.txt \
  --generate-notes
```

### 🖆 Edit a release

```bash
gh release edit v1.0.0 --title "Updated Release"
```

### 🖆 Delete a release

```bash
gh release delete v1.0.0
```

Deleting a release is different from deleting its Git tag. Check the command's available flags if you also intend to delete the tag.

# 🗁 Gist Management

GitHub Gists allow you to share small code snippets, notes, and configuration examples.

GitHub CLI provides the `gist` command group.

### 🖆 Create a public gist

```bash
gh gist create example.js
```

### 🖆 Create a secret gist

```bash
gh gist create example.js --secret
```

A secret gist is not listed publicly, but anyone with its URL can access it.

### 🖆 Add a description

```bash
gh gist create example.js --desc "JavaScript example"
```

### 🖆 List your gists

```bash
gh gist list
```

### 🖆 View a gist

```bash
gh gist view GIST_ID
```

### 🖆 Edit a gist

```bash
gh gist edit GIST_ID
```

### 🖆 Delete a gist

```bash
gh gist delete GIST_ID
```

# 🗁 Search GitHub

GitHub CLI supports searching repositories, issues, pull requests, and code.

### 🖆 Search repositories

```bash
gh search repos "react dashboard"
```

Filter by language:

```bash
gh search repos "dashboard" --language javascript
```

### 🖆 Search issues

```bash
gh search issues "login bug"
```

### 🖆 Search pull requests

```bash
gh search prs "authentication"
```

### 🖆 Search code

```bash
gh search code "useState("
```

Code search may require additional authentication permissions and supports GitHub's code-search syntax.

For more advanced filtering, consult the GitHub CLI search command documentation.

# 🗁 Notifications

GitHub CLI can retrieve notifications for your account.

List notifications:

```bash
gh notification list
```

Filter unread notifications:

```bash
gh notification list --unread
```

Mark notifications as read:

```bash
gh notification mark-read
```

Notification availability and filtering options depend on the installed CLI version.

# 🗁 Working with JSON Output

Many GitHub CLI commands support JSON output.

This is useful when building scripts, extracting information, or integrating GitHub data into other tools.

### 🖆 Display repository information as JSON

```bash
gh repo view --json name,description,url
```

Example output:

```json
{
  "name": "my-project",
  "description": "A development project",
  "url": "https://github.com/username/my-project"
}
```

### 🖆 Extract a specific field

Use `--jq` to filter JSON output:

```bash
gh repo view --json name --jq '.name'
```

Example output:

```text
my-project
```

### 🖆 List pull-request titles

```bash
gh pr list --json title --jq '.[].title'
```

### 🖆 Get issue numbers and titles

```bash
gh issue list --json number,title --jq '.[] | "\(.number): \(.title)"'
```

### 🖆 Use JSON output in shell scripts

```bash
gh pr list --json number,title,state
```

For scripts, prefer explicit JSON fields rather than parsing human-readable terminal output.

# 🗁 Aliases

Aliases allow you to create shorter commands for frequently used operations.

### 🖆 Create an alias

```bash
gh alias set pv 'pr view'
```

Use it:

```bash
gh pv 25
```

### 🖆 Create a repository-list alias

```bash
gh alias set repos 'repo list'
```

Use it:

```bash
gh repos
```

### 🖆 List aliases

```bash
gh alias list
```

### 🖆 Delete an alias

```bash
gh alias delete pv
```

Aliases can make repetitive GitHub CLI workflows more efficient.

# 🗁 Extensions

GitHub CLI supports extensions that add additional commands and functionality.

Extensions can be created by the community and installed from GitHub repositories.

### 🖆 Browse available extensions

```bash
gh extension browse
```

### 🖆 Install an extension

```bash
gh extension install OWNER/REPOSITORY
```

### 🖆 List installed extensions

```bash
gh extension list
```

### 🖆 Upgrade extensions

```bash
gh extension upgrade --all
```

### 🖆 Remove an extension

```bash
gh extension remove EXTENSION_NAME
```

Only install extensions from sources you trust. Extensions execute code on your machine and may access local resources or credentials available to the process.

# 🖆 Useful GitHub CLI Commands

| Command | Purpose |
| --- | --- |
| `gh auth login` | Authenticate with GitHub |
| `gh auth status` | Check authentication status |
| `gh auth logout` | Log out |
| `gh auth switch` | Switch authenticated accounts |
| `gh repo list` | List repositories |
| `gh repo view` | View repository details |
| `gh repo create` | Create a repository |
| `gh repo clone` | Clone a repository |
| `gh repo fork` | Fork a repository |
| `gh issue list` | List issues |
| `gh issue create` | Create an issue |
| `gh issue view` | View an issue |
| `gh pr list` | List pull requests |
| `gh pr create` | Create a pull request |
| `gh pr checkout` | Check out a pull request |
| `gh pr review` | Review a pull request |
| `gh pr merge` | Merge a pull request |
| `gh run list` | List workflow runs |
| `gh run view` | View a workflow run |
| `gh run watch` | Watch workflow progress |
| `gh workflow list` | List workflows |
| `gh workflow run` | Run a workflow manually |
| `gh release list` | List releases |
| `gh release create` | Create a release |
| `gh gist create` | Create a gist |
| `gh search repos` | Search repositories |
| `gh alias set` | Create a command alias |

# 🖆 Practical Workflows

### 🖆 Workflow: Create and publish a repository

Create a new local project:

```bash
mkdir my-project
cd my-project
git init
```

Create a README:

```bash
echo "# My Project" > README.md
```

Commit the initial files:

```bash
git add .
git commit -m "Initial commit"
```

Create a GitHub repository and push the project:

```bash
gh repo create my-project --public --source=. --remote=origin --push
```

This command creates the remote repository, configures the `origin` remote, and pushes the local repository.

### 🖆 Workflow: Create an issue and a feature branch

Create an issue:

```bash
gh issue create \
  --title "Add user authentication" \
  --body "Implement the login and registration functionality."
```

Create a feature branch:

```bash
git switch -c feature/user-authentication
```

Implement and commit the changes:

```bash
git add .
git commit -m "Add user authentication"
```

Push the branch:

```bash
git push -u origin feature/user-authentication
```

Create a pull request:

```bash
gh pr create \
  --title "Add user authentication" \
  --body "Implements login and registration functionality."
```

### 🖆 Workflow: Review and merge a pull request

List pull requests:

```bash
gh pr list
```

Check out a pull request:

```bash
gh pr checkout 25
```

Review its changes:

```bash
gh pr diff 25
```

Approve the pull request:

```bash
gh pr review 25 --approve
```

Merge it:

```bash
gh pr merge 25 --squash --delete-branch
```

### 🖆 Workflow: Monitor CI/CD

List recent workflow runs:

```bash
gh run list
```

View a workflow run:

```bash
gh run view RUN_ID
```

Watch the workflow:

```bash
gh run watch RUN_ID
```

If a workflow fails, inspect its logs:

```bash
gh run view RUN_ID --log
```

Rerun failed jobs:

```bash
gh run rerun RUN_ID --failed
```

### 🖆 Workflow: Create a versioned release

Create and push a Git tag:

```bash
git tag -a v1.0.0 -m "Version 1.0.0"
git push origin v1.0.0
```

Create the GitHub Release:

```bash
gh release create v1.0.0 --generate-notes
```

This combines Git versioning with GitHub's release management.

# 🖆 Troubleshooting

### 🖆 `gh: command not found`

Error:

```text
gh: command not found
```

Possible causes:

* GitHub CLI is not installed.
* Installation failed.
* The executable directory is not included in `PATH`.

Verify installation:

```bash
gh --version
```

Install GitHub CLI using the official installation instructions and restart your terminal if necessary.

### 🖆 Authentication required

Error:

```text
You are not logged into any GitHub hosts.
```

Solution:

```bash
gh auth login
```

Verify:

```bash
gh auth status
```

### 🖆 Authentication token is invalid

If your stored credentials are expired or invalid, authenticate again:

```bash
gh auth refresh
```

If necessary, log out and log in again:

```bash
gh auth logout
gh auth login
```

### 🖆 Repository not found

Possible causes:

* Incorrect repository owner or name.
* Repository is private and your account lacks access.
* You are authenticated with the wrong GitHub account.
* The repository URL is incorrect.

Check authentication:

```bash
gh auth status
```

Check repository access:

```bash
gh repo view OWNER/REPOSITORY
```

### 🖆 Pull request creation fails

Possible causes:

* The current branch has not been pushed.
* No commits exist between the base and head branches.
* The repository has no configured remote.
* The current account lacks permission.

Check the current branch:

```bash
git branch --show-current
```

Check remotes:

```bash
git remote -v
```

Push the branch:

```bash
git push -u origin HEAD
```

Then try creating the pull request again.

### 🖆 Workflow cannot be triggered manually

If `gh workflow run` fails, verify:

* The workflow file exists in `.github/workflows/`.
* The workflow defines the `workflow_dispatch` event.
* The workflow is enabled.
* Your account has sufficient repository permissions.

### 🖆 Inspect command options

When a command does not behave as expected, use its built-in help:

```bash
gh help
gh pr create --help
gh workflow run --help
```

# 🖆 Best Practices

* Keep Git and GitHub CLI updated.
* Authenticate using the appropriate GitHub account.
* Use SSH or HTTPS according to your environment.
* Avoid hardcoding access tokens in scripts.
* Use secure credential storage.
* Prefer JSON output for automation.
* Use explicit flags in scripts to reduce interactive prompts.
* Review destructive commands before executing them.
* Use aliases for frequently repeated operations.
* Install extensions only from trusted sources.
* Use GitHub Actions permissions according to the principle of least privilege.
* Use the official CLI documentation to verify command options for your installed version.

# 🖆 Practical Exercises

### 🖆 Exercise: Install GitHub CLI

Install GitHub CLI on your operating system.

Verify:

```bash
gh --version
```

### 🖆 Exercise: Authenticate

Authenticate with your GitHub account:

```bash
gh auth login
```

Check:

```bash
gh auth status
```

### 🖆 Exercise: Repository management

List your repositories:

```bash
gh repo list
```

Create a test repository:

```bash
gh repo create cli-practice --public --clone
```

### 🖆 Exercise: Issues management

Create an issue:

```bash
gh issue create \
  --title "Practice GitHub CLI" \
  --body "Testing issue management from the terminal."
```

List issues:

```bash
gh issue list
```

### 🖆 Exercise: Pull-request workflow

Create a feature branch, commit a change, push it, and create a pull request using GitHub CLI.

```bash
git switch -c feature/cli-practice
```

```bash
git add .
git commit -m "Practice GitHub CLI"
git push -u origin feature/cli-practice
```

```bash
gh pr create --title "Practice GitHub CLI" --body "Testing pull request creation."
```

### 🖆 Exercise: GitHub Actions

List workflow runs:

```bash
gh run list
```

Inspect a run:

```bash
gh run view RUN_ID
```

### 🖆 Exercise: Release management

Create a version tag and publish a GitHub Release:

```bash
git tag -a v1.0.0 -m "Initial release"
git push origin v1.0.0
gh release create v1.0.0 --generate-notes
```

# 🖆 Interview Questions

**What is GitHub CLI?**

GitHub CLI is an official command-line tool for interacting with GitHub resources such as repositories, issues, pull requests, releases, and Actions.

**What is the difference between Git and GitHub CLI?**

Git manages version control and repository history, while GitHub CLI manages GitHub platform features through the terminal.

**How do you authenticate with GitHub CLI?**

Use:

```bash
gh auth login
```

**How do you check authentication status?**

```bash
gh auth status
```

**How do you create a GitHub repository using CLI?**

```bash
gh repo create my-project --public
```

**How do you clone a GitHub repository using CLI?**

```bash
gh repo clone username/repository
```

**How do you create an issue using GitHub CLI?**

```bash
gh issue create --title "Bug report" --body "Describe the problem."
```

**How do you create a pull request?**

```bash
gh pr create --title "Feature update" --body "Description of changes."
```

**How do you merge a pull request using GitHub CLI?**

```bash
gh pr merge 25 --squash
```

**How do you monitor GitHub Actions workflows?**

```bash
gh run list
gh run view RUN_ID
gh run watch RUN_ID
```

**What is the purpose of `--json`?**

It returns structured JSON data that can be processed by scripts and other tools.

**What are GitHub CLI extensions?**

Extensions are additional commands that expand GitHub CLI functionality and are installed from extension repositories.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

* [GitHub CLI Official Website](https://cli.github.com/)
* [GitHub CLI Documentation](https://cli.github.com/manual/)
* [GitHub CLI Installation](https://github.com/cli/cli#installation)
* [GitHub CLI Repository](https://github.com/cli/cli)
* [GitHub CLI Authentication](https://cli.github.com/manual/gh_auth)
* [GitHub CLI Repository Commands](https://cli.github.com/manual/gh_repo)
* [GitHub CLI Issue Commands](https://cli.github.com/manual/gh_issue)
* [GitHub CLI Pull Request Commands](https://cli.github.com/manual/gh_pr)
* [GitHub CLI Actions Commands](https://cli.github.com/manual/gh_run)
* [GitHub CLI Release Commands](https://cli.github.com/manual/gh_release)
