# GitHub Basics

GitHub is a platform for hosting Git repositories and collaborating on software development projects.

It provides tools that help developers:

* Store and manage source code.
* Collaborate with other developers.
* Review and discuss code changes.
* Track bugs and feature requests.
* Automate development workflows.
* Publish releases and documentation.
* Contribute to open-source projects.

GitHub is widely used by individual developers, development teams, and organizations.

# 🗁 What Is GitHub?

**GitHub is a cloud-based development platform built around Git.**

It provides remote repository hosting along with collaboration, project management, automation, and code review features.

For example, a developer can:

* Create a project locally.
* Track changes using Git.
* Push the repository to GitHub.
* Share the repository with other developers.
* Collaborate through branches and pull requests.
* Automate testing using GitHub Actions.

A GitHub repository can contain:

* Source code.
* Documentation.
* Configuration files.
* Images and other project assets.
* Issue discussions.
* Pull requests.
* Releases.
* Automated workflows.

GitHub website:

<https://github.com/>

# 🗁 Git vs GitHub

Git and GitHub are related, but they are not the same thing.

| Git | GitHub |
| --- | --- |
| Version control system | Cloud-based development platform |
| Installed and used locally | Primarily accessed through a web interface and APIs |
| Tracks changes in files | Hosts Git repositories |
| Supports branches and commits | Provides collaboration around branches and commits |
| Works without an internet connection for local operations | Requires network access for hosted services |
| Open-source software | Commercial platform with free and paid offerings |

### 🖆 How They Work Together

```text
Developer
    |
    v
Local Project
    |
    v
Git
    |
    | Tracks changes
    | Creates commits
    | Manages branches
    |
    v
Local Git Repository
    |
    | git push
    v
GitHub Remote Repository
    |
    | Collaboration
    | Code Review
    | Issues
    | Pull Requests
    | Automation
    v
Other Developers
```

**Important:** Git performs version control. GitHub provides a platform for hosting repositories and collaborating around them.

# 🗁 Why Use GitHub?

GitHub provides several benefits for software development.

### 🖆 Code Hosting

Store Git repositories remotely so that projects can be accessed from different computers and locations.

### 🖆 Collaboration

Multiple developers can work on the same project using branches, pull requests, and code reviews.

### 🖆 Version History

Git preserves the project's commit history, while GitHub makes that history accessible through its interface.

### 🖆 Code Review

Developers can review changes, leave comments, request modifications, and approve pull requests.

### 🖆 Project Management

Use issues, projects, labels, milestones, and discussions to organize development work.

### 🖆 Automation

GitHub Actions can automate tasks such as:

* Running tests.
* Building applications.
* Checking code quality.
* Deploying applications.
* Publishing packages.

### 🖆 Open-Source Contributions

Discover public repositories, report issues, improve documentation, fix bugs, and contribute new features.

### 🖆 Portfolio

A GitHub profile can showcase projects, contributions, and technical skills.

# 🗁 Core GitHub Features

| Feature | Purpose |
| --- | --- |
| Repositories | Host and manage project files |
| Branches | Develop features independently |
| Commits | Record project changes |
| Pull Requests | Propose and review changes |
| Issues | Track bugs and feature requests |
| Discussions | Enable community conversations |
| Actions | Automate development workflows |
| Releases | Publish project versions |
| Projects | Organize and track work |
| Wiki | Maintain project documentation |
| Packages | Host software packages and container images |
| Pages | Publish websites directly from repositories |
| Gists | Share small code snippets and files |

These features are explored in more detail throughout the GitHub section.

# 🗁 Understanding GitHub Terminology

Understanding GitHub terminology makes collaboration easier.

### 🖆 Repository

A repository is a project workspace containing files, folders, and Git version history.

Example:

```text
my-web-app/
├── src/
├── public/
├── package.json
├── README.md
└── .gitignore
```

### 🖆 Fork

A fork is a personal copy of another user's repository under your own GitHub account or organization.

Forks are commonly used when contributing to projects where you do not have direct write access.

### 🖆 Star

A star is a way to bookmark or show appreciation for a repository.

### 🖆 Watch

Watching a repository allows you to receive notifications about selected repository activity.

### 🖆 Issue

An issue is used to track a bug, feature request, question, or other piece of work.

### 🖆 Pull Request (PR)

A pull request proposes changes from one branch into another and provides a place for discussion and review.

### 🖆 Discussion

Discussions provide a forum-like space for questions, announcements, ideas, and community conversations.

### 🖆 Release

A release represents a published version of a project, often associated with a Git tag and downloadable assets.

### 🖆 Organization

An organization is a shared GitHub account that allows multiple people to manage repositories and collaborate as a team.

### 🖆 Collaborator

A collaborator is someone who has been granted access to a repository.

### 🖆 Team

A team is a group of organization members that can be assigned permissions to repositories.

### 🖆 Contribution Graph

The contribution graph displays qualifying GitHub activity over time, such as commits and other contributions.

### 🖆 GitHub Actions

GitHub Actions is GitHub's automation platform for running workflows in response to repository events or scheduled triggers.

# 🗁 GitHub Account and Profile

A GitHub account provides access to GitHub's features and services.

Create an account:

<https://github.com/signup>

### 🖆 Profile Components

A GitHub profile can contain:

* Username.
* Profile picture.
* Display name.
* Biography.
* Location.
* Website.
* Social links.
* Pinned repositories.
* Contribution activity.
* Profile README.

Example profile URL:

```text
https://github.com/your-username
```

### 🖆 Choosing a Username

A good GitHub username should be:

* Professional.
* Easy to remember.
* Easy to spell.
* Consistent with your developer identity.

### 🖆 Profile Settings

GitHub profile settings allow you to manage personal information, public profile details, account preferences, security, and notifications.

Settings:

<https://github.com/settings/profile>

# 🗁 Understanding the GitHub Interface

The GitHub interface provides access to repositories, collaboration tools, account settings, and project activity.

<text-placeholder/>

### 🖆 Dashboard

The dashboard is the main landing area after signing in.

It provides access to repository activity, navigation, and relevant updates.

### 🖆 Repositories

The repositories section lists repositories that you own or have access to.

Example:

```text
https://github.com/your-username?tab=repositories
```

### 🖆 Pull Requests

The pull request interface lets you review proposed code changes, inspect differences, discuss implementation details, and manage approvals.

### 🖆 Issues

The issues interface helps track bugs, tasks, feature requests, and project discussions.

### 🖆 Explore

GitHub Explore helps discover repositories, developers, topics, and projects.

### 🖆 Notifications

Notifications provide updates about activity in repositories and conversations you follow or participate in.

### 🖆 Settings

Settings allow you to manage:

* Profile information.
* Account security.
* SSH and GPG keys.
* Applications.
* Access permissions.
* Notifications.
* Billing and plans.

# 🗁 Repositories: Public vs Private

GitHub supports different repository visibility settings.

### 🖆 Public Repository

A public repository is accessible to everyone.

Anyone can:

* View its files.
* Read its documentation.
* Clone it.
* Fork it, subject to applicable platform restrictions and repository settings.

Public visibility does not automatically grant permission to push changes.

### 🖆 Private Repository

A private repository is accessible only to authorized users.

It is commonly used for:

* Private application development.
* Commercial projects.
* Internal company code.
* Personal projects that should not be publicly accessible.

### 🖆 Comparison

| Feature | Public | Private |
| --- | --- | --- |
| Visible to everyone | Yes | No |
| Clone access | Generally available to everyone | Authorized users only |
| Push access | Requires permission | Requires permission |
| Suitable for open source | Yes | Usually no |
| Suitable for confidential code | No | Yes, with appropriate access controls |

### 🖆 Choosing Repository Visibility

Use public repositories when:

* Building an open-source project.
* Sharing educational resources.
* Publishing portfolio projects.
* Collaborating openly.

Use private repositories when:

* Working on confidential projects.
* Developing internal applications.
* Storing proprietary source code.
* Limiting access to selected collaborators.

**Important:** Repository visibility does not replace proper secret management. Never commit passwords, API keys, private tokens, or other credentials into a repository.

# 🗁 Personal Accounts vs Organizations

GitHub supports personal accounts and organizations.

### 🖆 Personal Account

A personal account represents an individual developer.

It can own repositories, contribute to projects, and collaborate with other users.

### 🖆 Organization

An organization represents a group, company, or community.

Organizations can manage:

* Shared repositories.
* Members.
* Teams.
* Repository permissions.
* Organization-level security settings.
* Billing and administrative policies.

### 🖆 Comparison

| Personal Account | Organization |
| --- | --- |
| Represents an individual | Represents a group |
| Owns personal repositories | Owns shared repositories |
| Managed by an individual | Managed by organization owners |
| Suitable for personal projects | Suitable for teams and companies |

Example:

```text
Personal Account:
github.com/alex

Organization:
github.com/company-name
```

# 🗁 GitHub Workflow Overview

A typical GitHub development workflow looks like this:

```text
Create Repository
        |
        v
Clone Repository
        |
        v
Create Feature Branch
        |
        v
Make Code Changes
        |
        v
Commit Changes
        |
        v
Push Branch to GitHub
        |
        v
Create Pull Request
        |
        v
Review Code
        |
        v
Merge Pull Request
        |
        v
Update Local Repository
```

### 🖆 Example Workflow

Clone a repository:

```bash
git clone https://github.com/username/project.git
```

Enter the project directory:

```bash
cd project
```

Create a feature branch:

```bash
git switch -c feature/user-authentication
```

Make changes and stage them:

```bash
git add .
```

Create a commit:

```bash
git commit -m "Add user authentication"
```

Push the branch:

```bash
git push -u origin feature/user-authentication
```

After pushing, create a pull request through GitHub's interface or an appropriate GitHub integration.

# 🗁 GitHub for Open Source

GitHub is one of the major platforms for open-source software development.

Open-source projects make their source code available under licenses that define how others may use, modify, and distribute it.

### 🖆 Common Ways to Contribute

* Report bugs.
* Suggest improvements.
* Fix existing issues.
* Improve documentation.
* Add tests.
* Review pull requests.
* Answer questions.
* Improve project accessibility.

### 🖆 Typical Contribution Workflow

```text
Find Open-Source Project
        |
        v
Read Contribution Guidelines
        |
        v
Fork Repository
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
Commit and Push
        |
        v
Open Pull Request
        |
        v
Maintainer Reviews Changes
        |
        v
Changes Merged or Revised
```

### 🖆 Before Contributing

Check these files and resources when available:

* `README.md` — Project overview and setup.
* `CONTRIBUTING.md` — Contribution guidelines.
* `CODE_OF_CONDUCT.md` — Community behavior expectations.
* `LICENSE` — Usage and distribution permissions.
* Issue templates — Expected bug reports or feature requests.

### 🖆 Contribution Etiquette

* Search existing issues before opening a duplicate.
* Follow the project's coding standards.
* Keep pull requests focused.
* Explain what changed and why.
* Include tests when appropriate.
* Respond constructively to review feedback.

# 🗁 Markdown on GitHub

GitHub supports Markdown for writing documentation, issue descriptions, pull requests, discussions, and other content.

Markdown helps make project documentation readable and organized.

### 🖆 Headings

```markdown
# Main Heading

# 🗁 Section Heading

### 🖆 Subsection Heading
```

### 🖆 Text Formatting

```markdown
**Bold text**

*Italic text*

~~Strikethrough text~~

`Inline code`
```

### 🖆 Lists

```markdown
- Item One
- Item Two
  - Nested Item
```

### 🖆 Links

```markdown
[GitHub Documentation](https://docs.github.com/)
```

### 🖆 Images

```markdown
![Image description](images/example.png)
```

### 🖆 Code Blocks

Use fenced code blocks with a language identifier:

````markdown
```javascript
const message = "Hello GitHub";

console.log(message);
```
````

### 🖆 Task Lists

```markdown
- [x] Create repository
- [x] Add README
- [ ] Configure GitHub Actions
```

### 🖆 Tables

```markdown
| Command | Description |
|---|---|
| git status | Show working tree status |
| git add . | Stage changes |
| git commit | Create a commit |
```

### 🖆 GitHub Flavored Markdown (GFM)

GitHub Flavored Markdown extends standard Markdown with additional features, including:

* Task lists.
* Tables.
* Strikethrough.
* Autolinks.
* Certain issue and pull request references.

Reference:

<https://github.github.com/gfm/>

# 🗁 GitHub Profile README

GitHub allows users to create a special repository that displays a README on their profile.

### 🖆 How It Works

Create a repository with exactly the same name as your GitHub username.

For example:

```text
GitHub Username:
developer123

Repository Name:
developer123
```

The repository must be public, and its README file must be located at the repository root.

GitHub can then display that README on your profile.

**Example Structure**

```text
developer123/
├── README.md
└── assets/
    └── profile-banner.png
```

### 🖆 What to Include

A useful profile README may contain:

* Short introduction.
* Technical skills.
* Programming languages.
* Frameworks and tools.
* Featured projects.
* Current learning goals.
* Contact and professional links.

Keep the information relevant and up to date.

# 🗁 GitHub Notifications and Watching

GitHub notifications help you stay informed about repository activity.

You may receive notifications when:

* Someone mentions you.
* Someone requests your review.
* A pull request receives comments.
* An issue you participate in is updated.
* A repository you watch has relevant activity.

### 🖆 Watch a Repository

Watching a repository lets you configure which notifications you receive about its activity.

The available notification preferences can include:

* Participating and mentions.
* All activity.
* Ignoring notifications.

The exact options depend on the repository and GitHub interface.

### 🖆 Notification Management

You can manage notification preferences through:

<https://github.com/settings/notifications>

Good notification management helps prevent unnecessary updates while keeping important conversations visible.

# 🗁 GitHub Security Basics

Protecting your GitHub account and repositories is essential.

### 🖆 Enable Two-Factor Authentication

Two-factor authentication (2FA) adds another verification step when signing in.

GitHub supports several authentication methods, including authenticator applications and security keys.

### 🖆 Use Passkeys Where Appropriate

Passkeys provide a phishing-resistant authentication option supported by compatible devices and browsers.

### 🖆 Protect Recovery Codes

Store recovery codes somewhere secure so you can regain access if your primary authentication method becomes unavailable.

### 🖆 Protect Access Tokens

Personal access tokens can grant access to GitHub resources.

Follow these practices:

* Never commit tokens into repositories.
* Avoid sharing tokens.
* Use the minimum required permissions.
* Set expiration dates where appropriate.
* Revoke tokens that are no longer needed.

### 🖆 Keep Sensitive Information Out of Repositories

Avoid committing:

* API keys.
* Passwords.
* Private SSH keys.
* Database credentials.
* Production environment secrets.
* Private certificates.

Use environment variables and dedicated secret-management mechanisms instead.

### 🖆 Review Repository Access

Regularly review collaborators, organization membership, deploy keys, and connected applications.

Security documentation:

<https://docs.github.com/en/authentication>

# 🗁 Other Useful GitHub Features

### 🖆 GitHub Pages

GitHub Pages allows you to publish websites directly from a GitHub repository.

Common use cases include:

* Developer portfolios.
* Project documentation.
* Static websites.
* Documentation for open-source projects.

Official documentation:

<https://docs.github.com/en/pages>

### 🖆 GitHub Actions

GitHub Actions automates workflows such as testing, building, and deployment.

Example workflow location:

```text
.github/
└── workflows/
    └── ci.yml
```

A workflow can run automatically when code is pushed or a pull request is opened.

### 🖆 GitHub Codespaces

Codespaces provides cloud-hosted development environments that can be configured for a repository.

It helps developers work on projects without manually configuring every development dependency on their local machine.

### 🖆 GitHub CLI

GitHub CLI (`gh`) is a command-line tool for interacting with GitHub.

Example:

```bash
gh auth login
```

List repositories:

```bash
gh repo list
```

Create a repository:

```bash
gh repo create my-project
```

GitHub CLI is covered in detail in `08-cli.md`.

# 🗁 Common GitHub Workflows

### 🖆 Create a Repository Through the GitHub Website

* Sign in to GitHub.
* Open the repository creation interface.
* Enter a repository name.
* Add an optional description.
* Choose public or private visibility.
* Optionally initialize the repository with a README.
* Create the repository.

### 🖆 Connect an Existing Local Repository to GitHub

Create an empty repository on GitHub, then connect it to your existing local repository:

```bash
git remote add origin https://github.com/username/project.git
```

Check the remote:

```bash
git remote -v
```

Push the current branch:

```bash
git push -u origin main
```

If your local branch has a different name, replace `main` with the actual branch name.

### 🖆 Clone a GitHub Repository

```bash
git clone https://github.com/username/project.git
```

This downloads the repository and its Git history to your local machine.

### 🖆 Update Your Local Repository

```bash
git pull origin main
```

This fetches remote changes and integrates them into the current branch.

# 🖆 Common Problems and Troubleshooting

### 🖆 Repository Not Found

Possible causes:

* Repository URL is incorrect.
* Repository was deleted or renamed.
* Repository is private and you lack access.
* Authentication is not configured correctly.

Check the remote URL:

```bash
git remote -v
```

### 🖆 Permission Denied

Possible causes:

* You do not have write access.
* Authentication credentials are incorrect.
* Your SSH key or personal access token is not configured correctly.
* The organization has additional access requirements.

Verify your repository permissions and authentication configuration.

### 🖆 Push Rejected

A push can be rejected when the remote branch contains changes that are missing locally.

Try:

```bash
git pull --rebase origin main
```

Resolve any conflicts, then push again:

```bash
git push origin main
```

Use the appropriate branch name for your workflow.

### 🖆 Forgot GitHub Password

Use GitHub's account recovery process:

<https://github.com/password_reset>

### 🖆 Cannot See Contributions on Profile

Possible reasons include:

* Commits use an email address not associated with your GitHub account.
* Commits are on a repository or branch that does not meet contribution graph requirements.
* Contributions have not yet been reflected in the profile.
* The repository's visibility affects how contributions are displayed.

Check the official contribution documentation:

<https://docs.github.com/en/account-and-profile/concepts/contributions-on-your-profile>

# 🖆 Best Practices

* Use a professional GitHub username and profile.
* Write meaningful repository names and descriptions.
* Add a clear `README.md` to every project.
* Choose repository visibility carefully.
* Keep repositories organized.
* Use branches for independent development.
* Write descriptive commit messages.
* Use pull requests for code review.
* Protect important branches with appropriate rules.
* Never commit secrets or credentials.
* Enable strong account authentication.
* Keep project documentation updated.
* Follow contribution guidelines when working with open-source projects.
* Review repository permissions regularly.
* Use GitHub Actions to automate repetitive development tasks.

# 🖆 Practical Exercises

### 🖆 Exercise: Explore GitHub

* [ ] Visit <https://github.com/>.
* [ ] Create a GitHub account.
* [ ] Explore the GitHub dashboard.
* [ ] Open your profile page.
* [ ] Explore the repository section.
* [ ] Find an open-source project.
* [ ] Explore its README, issues, pull requests, and releases.
* [ ] Star a repository that you find useful.

### 🖆 Exercise: Create Your First Repository

* [ ] Create a repository named `github-learning`.
* [ ] Add a description.
* [ ] Choose public or private visibility.
* [ ] Initialize it with a README.
* [ ] Add a `notes.md` file.
* [ ] Edit the README through GitHub.
* [ ] Review the commit history.

### 🖆 Exercise: Work With a Local Repository

* [ ] Clone your GitHub repository.
* [ ] Create a feature branch.
* [ ] Add a new Markdown file.
* [ ] Commit your changes.
* [ ] Push your branch.
* [ ] Open a pull request.
* [ ] Review the changes.
* [ ] Merge the pull request.
* [ ] Update your local `main` branch.

### 🖆 Exercise: Explore Open Source

* [ ] Find a project related to your interests.
* [ ] Read its README.
* [ ] Review its contribution guidelines.
* [ ] Explore open issues.
* [ ] Identify a small documentation improvement.
* [ ] Fork the repository.
* [ ] Create a branch and make a small change.
* [ ] Submit a pull request if the project accepts the contribution.

# 🖆 Interview Questions

**What is GitHub?**

GitHub is a cloud-based platform for hosting Git repositories and collaborating on software development projects.

**What is the difference between Git and GitHub?**

Git is a distributed version control system, while GitHub is a platform that hosts Git repositories and provides collaboration tools.

**What is a GitHub repository?**

A GitHub repository is a hosted project workspace containing files and Git version history.

**What is the difference between a public and private repository?**

A public repository is visible to everyone, while a private repository is accessible only to authorized users.

**What is a fork?**

A fork is a copy of another repository under a different GitHub account or organization.

**What is a star on GitHub?**

A star is a way to bookmark or show appreciation for a repository.

**What is watching a repository?**

Watching a repository allows users to receive notifications about selected repository activity.

**What is a pull request?**

A pull request proposes changes from one branch into another and provides a place for code review and discussion.

**What is an issue?**

An issue tracks bugs, feature requests, tasks, or questions related to a project.

**What is a GitHub organization?**

An organization is a shared GitHub account that allows multiple users to manage repositories and collaborate.

**What is GitHub Actions?**

GitHub Actions is an automation platform used to run workflows for tasks such as testing, building, and deploying applications.

**What is a GitHub Profile README?**

It is a special README displayed on a user's profile when a public repository with the same name as the username contains a root-level README file.

**Can Git be used without GitHub?**

Yes. Git works locally and can also communicate with remote Git servers other than GitHub.

**Can GitHub host repositories without Git?**

GitHub repositories use Git for version control. GitHub also provides other platform features, but its repository version history is based on Git.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

* [GitHub](https://github.com/)
* [GitHub Documentation](https://docs.github.com/)
* [Getting Started with GitHub](https://docs.github.com/en/get-started)
* [GitHub Account and Profile](https://docs.github.com/en/account-and-profile)
* [GitHub Repositories](https://docs.github.com/en/repositories)
* [Learning About GitHub](https://docs.github.com/en/get-started/learning-about-github)
* [Exploring Projects on GitHub](https://docs.github.com/en/get-started/exploring-projects-on-github)
* [GitHub Skills](https://skills.github.com/)
* [GitHub Flavored Markdown](https://github.github.com/gfm/)
