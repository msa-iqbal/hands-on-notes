# GitHub Issues

GitHub Issues is a built-in issue-tracking system used to organize, discuss, and manage work within a repository.

It helps development teams track:

* Software bugs.
* Feature requests.
* Improvements.
* Documentation tasks.
* Technical questions.
* Maintenance work.
* Development discussions.

Issues provide a central place where developers can describe problems, assign responsibilities, discuss solutions, and track progress.

For example, a web application project might use GitHub Issues to track:

```text
Issue #12: Fix login validation

Issue #13: Add password reset functionality

Issue #14: Improve dashboard loading performance

Issue #15: Update installation documentation
```

# 🗁 What Are GitHub Issues?

A GitHub Issue is an individual record used to track a task, problem, idea, or discussion related to a project.

An issue typically contains:

* Title.
* Description.
* Author.
* Assignees.
* Labels.
* Milestone.
* Comments.
* References to commits and pull requests.
* Status and activity history.

Example issue URL:

```text
https://github.com/username/project/issues/12
```

Each issue receives a unique number within its repository.

For example:

```text
#12
#13
#14
```

Issue numbers make it easy to reference specific tasks throughout the project.

# 🗁 Why Use GitHub Issues?

GitHub Issues provides several benefits for software development.

### 🖆 Bug Tracking

Record software defects and track their resolution.

Example:

```text
Bug: Login fails with valid credentials

Expected:
User should successfully log in.

Actual:
The application displays an authentication error.
```

### 🖆 Feature Requests

Collect ideas and requests for new functionality.

Example:

```text
Feature: Add dark mode

Description:
Allow users to switch between light and dark themes.
```

### 🖆 Task Management

Break development work into smaller, manageable tasks.

Example:

```text
Task: Implement user authentication

- Create login API
- Create registration API
- Add password validation
- Implement authentication middleware
- Write tests
```

### 🖆 Team Collaboration

Developers can discuss implementation details, assign tasks, and share updates directly within issues.

### 🖆 Project Organization

Labels, milestones, assignments, and GitHub Projects help teams organize and prioritize work.

# 🗁 Understanding an Issue

A typical GitHub Issue contains several important components.

| Component | Description |
| --- | --- |
| Title | Short summary of the issue |
| Description | Detailed explanation |
| Author | User who created the issue |
| Assignees | People responsible for the work |
| Labels | Categories used to organize issues |
| Milestone | Project goal or release grouping |
| Comments | Discussion and updates |
| Linked items | Related issues, pull requests, and other work |
| State | Open or closed |

Example:

```text
Issue #24

Title:
Fix responsive navigation menu

Author:
developer123

Assignees:
frontend-developer

Labels:
bug, frontend, high-priority

Milestone:
v1.2.0

State:
Open
```

# 🗁 Creating an Issue

Issues can be created through the GitHub website or GitHub CLI.

### 🖆 Create an Issue Through GitHub

Open the repository:

```text
Repository → Issues → New issue
```

Then:

* Enter a descriptive title.
* Write the issue description.
* Select an issue template if available.
* Assign the issue if appropriate.
* Add relevant labels.
* Associate a milestone if applicable.
* Submit the issue.

### 🖆 Create an Issue Using GitHub CLI

Authenticate first:

```bash
gh auth login
```

Create an issue interactively:

```bash
gh issue create
```

Create an issue with a title and body:

```bash
gh issue create \
  --title "Fix login validation" \
  --body "Login validation fails for valid credentials."
```

Create an issue with labels:

```bash
gh issue create \
  --title "Improve dashboard performance" \
  --body "Optimize dashboard loading time." \
  --label "performance"
```

Assign an issue:

```bash
gh issue create \
  --title "Update documentation" \
  --body "Improve installation instructions." \
  --assignee username
```

The assignee must have appropriate access to the repository.

# 🗁 Issue Title and Description

A good issue title should clearly communicate the problem or task.

### 🖆 Good Titles

```text
Fix authentication failure on expired tokens

Add pagination to the users endpoint

Improve mobile navigation layout

Update installation instructions
```

### 🖆 Poor Titles

```text
Problem

Something is broken

Please fix this

Help needed
```

### 🖆 Writing a Good Description

A bug report should contain enough information for someone else to reproduce and understand the problem.

Example:

```markdown
# 🗁 Description

Users cannot log in after their session expires.

# 🗁 Steps to Reproduce

- Log in to the application.
- Wait until the session expires.
- Refresh the page.
- Attempt to log in again.

# 🗁 Expected Behavior

The application should redirect users to the login page.

# 🗁 Actual Behavior

The application displays an authentication error.

# 🗁 Environment

- Browser: Chrome
- Operating System: Linux
- Application Version: 1.2.0

# 🗁 Additional Information

The issue appears after token expiration.
```

For a feature request:

```markdown
# 🗁 Feature Description

Add a dark mode option to the application.

# 🗁 Motivation

Users should be able to choose a theme that suits their preferences.

# 🗁 Proposed Solution

- Add a theme switcher.
- Support light and dark themes.
- Persist the selected theme.

# 🗁 Acceptance Criteria

- [ ] Users can switch between themes.
- [ ] The selected theme persists after refresh.
- [ ] All major application components support both themes.
```

# 🗁 Issue Types

GitHub supports different ways to categorize work.

Common issue categories include:

| Type | Purpose |
| --- | --- |
| Bug | Report incorrect or unexpected behavior |
| Feature | Request new functionality |
| Enhancement | Improve existing functionality |
| Documentation | Track documentation work |
| Task | Track a unit of work |
| Question | Ask for technical clarification |

The exact issue types available depend on repository or organization configuration.

Labels can also be used to classify issues.

For example:

```text
bug
feature
enhancement
documentation
question
maintenance
```

**Issue types and labels are different.** Issue types provide a higher-level classification, while labels are flexible tags that can represent categories, priorities, components, or workflows.

# 🗁 Issue States

GitHub Issues have two primary states:

* Open.
* Closed.

### 🖆 Open

An open issue is still active and may require discussion, implementation, or further work.

### 🖆 Closed

A closed issue has been completed, resolved, or intentionally closed.

Closing an issue does not necessarily mean the underlying problem was fixed. The issue may also be closed as duplicate, invalid, or not planned.

### 🖆 View Open Issues

```bash
gh issue list --state open
```

### 🖆 View Closed Issues

```bash
gh issue list --state closed
```

### 🖆 View All Issues

```bash
gh issue list --state all
```

# 🗁 Assigning Issues

Assignees are users responsible for working on an issue.

Assigning issues helps clarify ownership and accountability.

### 🖆 Assign Through GitHub

Open an issue and select the assignee section.

Choose one or more eligible users.

### 🖆 Assign Using GitHub CLI

```bash
gh issue edit 12 --add-assignee username
```

Remove an assignee:

```bash
gh issue edit 12 --remove-assignee username
```

Assign multiple users:

```bash
gh issue edit 12 --add-assignee username1,username2
```

### 🖆 Assignment Best Practices

* Assign issues to people who can reasonably work on them.
* Avoid assigning too many unrelated tasks to one person.
* Clarify responsibilities when multiple people are assigned.
* Update assignments when ownership changes.

# 🗁 Issue Labels

Labels categorize and organize issues.

They make it easier to filter and identify work.

### 🖆 Common Labels

| Label | Meaning |
| --- | --- |
| `bug` | Something is not working correctly |
| `feature` | New functionality |
| `enhancement` | Improvement to existing functionality |
| `documentation` | Documentation-related work |
| `good first issue` | Suitable for newcomers |
| `help wanted` | Additional contributors are needed |
| `priority: high` | High-priority work |
| `priority: low` | Low-priority work |
| `wontfix` | The issue will not be addressed |
| `duplicate` | The issue already exists |

### 🖆 Add a Label Through GitHub

Open an issue and use the Labels section to select relevant labels.

### 🖆 Add a Label Using GitHub CLI

```bash
gh issue edit 12 --add-label bug
```

Add multiple labels:

```bash
gh issue edit 12 --add-label bug,frontend
```

Remove a label:

```bash
gh issue edit 12 --remove-label frontend
```

### 🖆 Create a Label Using GitHub CLI

```bash
gh label create "priority: high" \
  --description "High-priority issue" \
  --color FF0000
```

List repository labels:

```bash
gh label list
```

### 🖆 Labeling Strategy

A useful labeling system can separate different dimensions:

```text
Type:
bug
feature
documentation

Priority:
priority: critical
priority: high
priority: medium
priority: low

Area:
frontend
backend
database
devops

Status:
needs-triage
blocked
in-progress
```

Avoid creating too many overlapping labels. Keep the system consistent and understandable.

# 🗁 Milestones

A milestone groups issues and pull requests around a project goal or target release.

For example:

```text
Milestone: v1.0.0

Issues:
- Implement authentication
- Add user dashboard
- Add password reset
- Write API documentation
```

Milestones help track progress toward a release or project objective.

### 🖆 Create a Milestone

Open:

```text
Repository → Issues → Milestones → New milestone
```

Enter:

* Title.
* Description.
* Optional due date.

Create the milestone.

### 🖆 Assign an Issue to a Milestone

Open an issue and select the appropriate milestone.

### 🖆 View Milestone Progress

GitHub calculates progress using the associated open and closed issues and pull requests.

### 🖆 Milestones Using GitHub CLI

List milestones:

```bash
gh api repos/OWNER/REPO/milestones
```

The GitHub CLI API command uses the authenticated account and the repository specified in the endpoint.

Milestones are useful for release planning, but they are not a complete replacement for project management tools.

# 🗁 Issue Comments and Mentions

Comments allow participants to discuss issues and provide updates.

### 🖆 Add a Comment

Open an issue, enter a comment in the discussion area, and submit it.

### 🖆 Mention a User

Use the `@` symbol followed by a GitHub username.

```text
@username Can you review this implementation?
```

Mentions can notify the referenced user.

### 🖆 Mention an Issue

Reference an issue using its number:

```text
#12
```

### 🖆 Mention a Pull Request

Pull requests can also be referenced by number:

```text
#25
```

### 🖆 Use Markdown in Comments

GitHub supports Markdown formatting in issue comments.

Example:

```markdown
**Update**

The authentication issue has been reproduced.

```bash
npm test
```

The failing test is related to expired tokens.

### 🖆 Comment Best Practices

* Keep comments relevant.
* Provide useful technical details.
* Avoid unnecessary notifications.
* Use mentions when a person's attention is needed.
* Summarize important decisions.

# 🗁 Issue References and Linking

GitHub allows issues to reference related development activity.

### 🖆 Reference an Issue

Use:

```text
#12
```

Within the same repository, GitHub recognizes this as a reference to issue or pull request number 12.

### 🖆 Reference an Issue in Another Repository

Use:

```text
owner/repository#12
```

Example:

```text
company/api-server#45
```

### 🖆 Link an Issue to a Pull Request

A pull request can include a closing keyword followed by an issue reference.

Common keywords include:

```text
close
closes
closed

fix
fixes
fixed

resolve
resolves
resolved
```

Example pull request description:

```markdown
# 🗁 Changes

- Added token validation.
- Improved error handling.
- Added authentication tests.

Closes #12
```

When the pull request is merged into the repository's default branch, GitHub automatically closes the referenced issue.

This automatic behavior depends on the closing keyword and repository relationship being supported.

### 🖆 Cross-Repository Issue References

A pull request can reference an issue in another repository:

```text
Fixes company/api-server#45
```

For cross-repository closing behavior, GitHub requires the appropriate repository relationship and permissions.

# 🗁 Closing and Reopening Issues

An issue can be closed when the work is completed or when the issue no longer requires action.

### 🖆 Close an Issue Through GitHub

Open the issue and select **Close issue**.

### 🖆 Close an Issue Using GitHub CLI

```bash
gh issue close 12
```

Add a closing comment:

```bash
gh issue close 12 \
  --comment "Fixed in the latest release."
```

### 🖆 Reopen an Issue

```bash
gh issue reopen 12
```

### 🖆 Close an Issue Through a Commit Message

GitHub can automatically close issues when a commit containing a supported closing keyword is pushed to the repository's default branch.

Example:

```bash
git commit -m "Fix login validation

Fixes #12"
```

A commit pushed to a feature branch does not necessarily close the issue immediately. The closing behavior is associated with the commit reaching the default branch through the supported workflow.

### 🖆 Close an Issue Through a Pull Request

Add this to the pull request description:

```text
Closes #12
```

When the pull request is merged into the default branch, the issue is automatically closed.

# 🗁 Issue Templates

Issue templates provide predefined structures for creating issues.

They help maintain consistent reports and collect relevant information.

### 🖆 Why Use Issue Templates?

Templates help:

* Standardize bug reports.
* Collect reproduction steps.
* Improve feature requests.
* Reduce incomplete reports.
* Make issue triage easier.

### 🖆 Template Directory

GitHub supports issue templates stored in:

```text
.github/
└── ISSUE_TEMPLATE/
    ├── bug-report.md
    ├── feature-request.md
    └── config.yml
```

### 🖆 Bug Report Template

Create:

`.github/ISSUE_TEMPLATE/bug-report.md`

```markdown
---
name: Bug Report
about: Report a problem
title: "[BUG] "
labels: bug
assignees: ''
---

# 🗁 Description

Describe the problem.

# 🗁 Steps to Reproduce

- Step one
- Step two
- Step three

# 🗁 Expected Behavior

Describe what should happen.

# 🗁 Actual Behavior

Describe what happens instead.

# 🗁 Environment

- Operating System:
- Browser:
- Application Version:

# 🗁 Additional Information

Add screenshots, logs, or other relevant details.
```

### 🖆 Feature Request Template

Create:

`.github/ISSUE_TEMPLATE/feature-request.md`

```markdown
---
name: Feature Request
about: Suggest a new feature
title: "[FEATURE] "
labels: enhancement
assignees: ''
---

# 🗁 Problem

Describe the problem this feature should solve.

# 🗁 Proposed Solution

Explain the suggested functionality.

# 🗁 Alternatives

Describe any alternative solutions.

# 🗁 Additional Context

Add relevant examples or screenshots.
```

### 🖆 Issue Template Configuration

The `config.yml` file can configure the issue creation experience.

Example:

```yaml
blank_issues_enabled: false

contact_links:
  - name: GitHub Discussions
    url: https://github.com/OWNER/REPO/discussions
    about: Ask questions and discuss ideas.
```

This example disables blank issue creation and provides a link to Discussions.

# 🗁 Issue Forms

Issue forms provide structured input fields for collecting issue information.

Unlike Markdown templates, issue forms use YAML configuration.

They support fields such as:

* Input.
* Textarea.
* Dropdown.
* Checkboxes.
* Markdown information blocks.

### 🖆 Issue Form Directory

```text
.github/
└── ISSUE_TEMPLATE/
    └── bug-report.yml
```

**Example Bug Report Form**

```yaml
name: Bug Report
description: Report a software bug.
title: "[BUG] "
labels:
  - bug
body:
  - type: markdown
    attributes:
      value: |
        Please provide enough information to reproduce the issue.

  - type: input
    id: summary
    attributes:
      label: Bug Summary
      description: Briefly describe the problem.
      placeholder: Login fails with valid credentials.
    validations:
      required: true

  - type: textarea
    id: reproduction
    attributes:
      label: Steps to Reproduce
      description: Explain how to reproduce the problem.
      placeholder: |
        - Open the login page.
        - Enter valid credentials.
        - Submit the form.
    validations:
      required: true

  - type: dropdown
    id: environment
    attributes:
      label: Environment
      options:
        - Development
        - Staging
        - Production
    validations:
      required: true

  - type: textarea
    id: expected
    attributes:
      label: Expected Behavior
    validations:
      required: true

  - type: textarea
    id: actual
    attributes:
      label: Actual Behavior
    validations:
      required: true
```

### 🖆 Markdown Template vs Issue Form

| Markdown Template | Issue Form |
| --- | --- |
| Written in Markdown | Configured using YAML |
| Provides a predefined text structure | Provides structured form fields |
| Users write directly into the description | Users fill out individual fields |
| Flexible and simple | More controlled input |
| Suitable for free-form reports | Suitable for standardized reports |

Use Markdown templates when flexibility is important. Use issue forms when consistent structured information is required.

# 🗁 Issue Dependencies

Some tasks depend on other tasks being completed first.

GitHub supports issue relationships and dependency tracking through supported issue linking features.

Example:

```text
Issue #20: Create authentication API
        |
        v
Issue #21: Build login interface
        |
        v
Issue #22: Integrate login API
```

Here, the integration task depends on the authentication API and login interface.

Dependency relationships help teams understand:

* Which tasks are blocked.
* Which tasks can begin independently.
* What must be completed first.
* How work is connected.

GitHub's issue dependency features and interface may vary by account and repository configuration.

For more complex planning, use GitHub Projects alongside issues.

# 🗁 GitHub Projects and Issues

GitHub Projects provides project-management capabilities that can organize issues and pull requests.

A project can include:

* Issues.
* Pull requests.
* Draft issues.
* Custom fields.
* Views.
* Filters.
* Grouping.
* Sorting.
* Automation.

### 🖆 Example Project Board

| Backlog | Todo | In Progress | Done |
| --- | --- | --- | --- |
| Add dark mode | Improve API | Fix login | Add README |
| Add search | Update tests | Refactor auth | Update dependencies |

### 🖆 Views

Projects can organize work through different views.

**Table view:** Displays work items in rows and columns.

**Board view:** Displays work items in columns, often based on status.

**Roadmap view:** Displays scheduled work along a timeline.

### 🖆 Issue vs Project

| GitHub Issues | GitHub Projects |
| --- | --- |
| Tracks individual work items | Organizes work across multiple items |
| Provides discussion and issue history | Provides planning and visualization |
| Supports assignments and labels | Supports custom fields and views |
| Focuses on individual tasks | Focuses on broader project coordination |

A GitHub Project can include issues from multiple repositories.

# 🗁 Managing Issues with GitHub CLI

GitHub CLI provides commands for working with issues from the terminal.

### 🖆 Authenticate

```bash
gh auth login
```

### 🖆 List Issues

```bash
gh issue list
```

List open issues:

```bash
gh issue list --state open
```

List closed issues:

```bash
gh issue list --state closed
```

### 🖆 View an Issue

```bash
gh issue view 12
```

Open the issue in a browser:

```bash
gh issue view 12 --web
```

### 🖆 Create an Issue

```bash
gh issue create
```

### 🖆 Edit an Issue

```bash
gh issue edit 12 \
  --title "Fix authentication validation"
```

Update the body:

```bash
gh issue edit 12 \
  --body "Update the authentication validation logic."
```

### 🖆 Add Labels

```bash
gh issue edit 12 --add-label bug
```

### 🖆 Assign an Issue

```bash
gh issue edit 12 --add-assignee username
```

### 🖆 Close an Issue

```bash
gh issue close 12
```

### 🖆 Reopen an Issue

```bash
gh issue reopen 12
```

### 🖆 Delete an Issue

GitHub does not provide a general-purpose issue deletion workflow for ordinary repository users through the standard issue management interface. Closing an issue is the normal way to remove it from active work.

### 🖆 Useful CLI Commands

| Command | Description |
| --- | --- |
| `gh issue list` | List issues |
| `gh issue view 12` | View an issue |
| `gh issue create` | Create an issue |
| `gh issue edit 12` | Edit an issue |
| `gh issue close 12` | Close an issue |
| `gh issue reopen 12` | Reopen an issue |
| `gh issue status` | View your issue and pull request status |

Reference:

<https://cli.github.com/manual/gh_issue>

# 🗁 Issue Management Workflow

A practical issue-management workflow looks like this:

```text
Identify Problem or Feature
          |
          v
Create GitHub Issue
          |
          v
Write Clear Description
          |
          v
Add Labels and Milestone
          |
          v
Assign Responsible Developer
          |
          v
Discuss and Clarify Requirements
          |
          v
Create Development Branch
          |
          v
Implement and Test Changes
          |
          v
Open Pull Request
          |
          v
Link Pull Request to Issue
          |
          v
Review and Merge
          |
          v
Issue Automatically Closes
```

### 🖆 Example Development Workflow

Create an issue:

```text
Title:
Add password reset functionality

Description:
Allow users to reset their passwords using an email verification link.
```

Create a development branch:

```bash
git switch -c feature/password-reset
```

Implement the feature and commit:

```bash
git add .

git commit -m "Add password reset functionality"
```

Push the branch:

```bash
git push -u origin feature/password-reset
```

Create a pull request:

```bash
gh pr create
```

Include this in the pull request description:

```text
Closes #24
```

When the pull request is merged into the default branch, GitHub closes issue `#24`.

# 🗁 Good Issue Writing Practices

A well-written issue should be understandable without requiring extensive clarification.

### 🖆 Use Specific Titles

Good:

```text
Fix API timeout when uploading large files
```

Poor:

```text
Upload problem
```

### 🖆 Explain the Problem

Describe what is happening and why it matters.

### 🖆 Provide Reproduction Steps

For bugs, include the steps required to reproduce the behavior.

### 🖆 Include Expected and Actual Results

Clearly explain the difference between expected behavior and actual behavior.

### 🖆 Add Relevant Evidence

Include:

* Error messages.
* Logs.
* Screenshots.
* Stack traces.
* Browser information.
* Environment details.

Remove secrets and sensitive information before sharing logs.

### 🖆 Define Acceptance Criteria

For feature requests, explain what must be true for the work to be considered complete.

### 🖆 Avoid Duplicate Issues

Search existing issues before creating a new one.

### 🖆 Keep Issues Focused

One issue should generally describe one coherent problem or unit of work.

Split unrelated tasks into separate issues.

# 🖆 Common Problems and Troubleshooting

### 🖆 Cannot Create an Issue

Possible causes:

* You do not have permission to create issues.
* Issues are disabled in repository settings.
* The repository is archived.
* An organization policy restricts issue creation.

Check repository features and your access permissions.

### 🖆 Cannot Assign an Issue

Possible causes:

* The selected user does not have the required repository access.
* The username is incorrect.
* Organization or repository policies restrict assignment.

Verify the user's access and repository membership.

### 🖆 Labels Are Not Available

Possible causes:

* The label does not exist.
* You do not have permission to manage labels.
* You are working in the wrong repository.

List labels using:

```bash
gh label list
```

### 🖆 Issue Does Not Close Automatically

Check:

* Whether the pull request contains a supported closing keyword.
* Whether the issue reference is correct.
* Whether the pull request targets the default branch.
* Whether the referenced issue belongs to the appropriate repository.
* Whether the pull request has actually been merged.

Example:

```text
Closes #12
```

### 🖆 Issue Template Is Not Appearing

Check:

* The directory is named `.github/ISSUE_TEMPLATE/`.
* The template file is committed to the repository's default branch.
* The file extension is supported.
* YAML front matter is valid for Markdown templates.
* YAML syntax is valid for issue forms.

### 🖆 GitHub CLI Authentication Fails

Check authentication:

```bash
gh auth status
```

Authenticate again if necessary:

```bash
gh auth login
```

# 🖆 Best Practices

* Use issues to track actionable work.
* Write clear and descriptive titles.
* Provide detailed descriptions.
* Use templates for consistent reports.
* Use issue forms for structured information.
* Apply labels consistently.
* Assign clear ownership.
* Use milestones for release planning.
* Link related issues and pull requests.
* Define acceptance criteria for feature requests.
* Close completed or invalid issues.
* Avoid duplicate and overly broad issues.
* Keep discussions focused and constructive.
* Use GitHub Projects for broader planning.
* Use GitHub CLI for efficient terminal-based management.

# 🖆 Practical Exercises

### 🖆 Exercise: Create and Manage Issues

* [ ] Open your GitHub practice repository.
* [ ] Create a bug report.
* [ ] Create a feature request.
* [ ] Add labels to both issues.
* [ ] Assign an issue to yourself.
* [ ] Add a comment.
* [ ] Mention another collaborator if available.
* [ ] Close one issue.
* [ ] Reopen it.

### 🖆 Exercise: Create Labels and Milestones

* [ ] Create a `bug` label.
* [ ] Create a `feature` label.
* [ ] Create a `documentation` label.
* [ ] Create a `priority: high` label.
* [ ] Create a milestone named `v1.0.0`.
* [ ] Assign multiple issues to the milestone.
* [ ] Review milestone progress.

### 🖆 Exercise: Create an Issue Template

* [ ] Create `.github/ISSUE_TEMPLATE/`.
* [ ] Add `bug-report.md`.
* [ ] Add `feature-request.md`.
* [ ] Add a template configuration file.
* [ ] Commit and push the templates.
* [ ] Open the new issue page.
* [ ] Verify that the templates appear.

### 🖆 Exercise: Create an Issue Form

* [ ] Create `bug-report.yml`.
* [ ] Add an input field.
* [ ] Add a textarea field.
* [ ] Add a dropdown field.
* [ ] Configure required fields.
* [ ] Commit the form.
* [ ] Test the issue creation interface.

### 🖆 Exercise: Link an Issue to a Pull Request

* [ ] Create an issue named `Add contact form`.
* [ ] Create a feature branch.
* [ ] Implement a small change.
* [ ] Commit and push the branch.
* [ ] Open a pull request.
* [ ] Add `Closes #ISSUE_NUMBER` to the description.
* [ ] Merge the pull request.
* [ ] Verify that the issue is closed.

### 🖆 Exercise: Use GitHub CLI

* [ ] Authenticate with `gh auth login`.
* [ ] List repository issues.
* [ ] View an issue.
* [ ] Create an issue.
* [ ] Add a label.
* [ ] Assign an issue.
* [ ] Close an issue.
* [ ] Reopen the issue.

# 🖆 Interview Questions

**What are GitHub Issues?**

GitHub Issues is a built-in issue-tracking system used to manage bugs, feature requests, tasks, and discussions.

**Why are GitHub Issues useful?**

They provide a central place to report problems, assign responsibilities, discuss solutions, and track development work.

**What information does an issue contain?**

An issue can contain a title, description, author, assignees, labels, milestone, comments, references, and state.

**What is the difference between an issue and a pull request?**

An issue tracks a problem or task. A pull request proposes code changes and provides a place for review and discussion.

**What are issue labels?**

Labels are tags used to categorize and filter issues.

**What is a milestone?**

A milestone groups issues and pull requests around a project objective or release.

**What is an issue assignee?**

An assignee is a user responsible for working on an issue.

**What is the difference between an issue template and an issue form?**

An issue template provides a predefined Markdown structure, while an issue form collects information through structured fields configured with YAML.

**How can a pull request automatically close an issue?**

Include a supported closing keyword, such as `Closes #12`, in the pull request description. When the pull request is merged into the default branch, GitHub closes the referenced issue.

**What is the purpose of GitHub Projects?**

GitHub Projects helps organize and track issues, pull requests, and other work items through views, fields, and project workflows.

**Can GitHub Issues be managed from the terminal?**

Yes. GitHub CLI provides commands such as `gh issue list`, `gh issue create`, `gh issue view`, and `gh issue close`.

**What is the difference between open and closed issues?**

Open issues are active and may require work. Closed issues are considered resolved or no longer active.

**How do you reference an issue in a commit message?**

Use its issue number, such as `#12`, or include a closing keyword such as `Fixes #12`.

**What makes a good bug report?**

A good bug report includes a clear description, reproduction steps, expected behavior, actual behavior, environment details, and relevant evidence.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**Official References**

* [About Issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues)
* [Creating an Issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue)
* [Managing Issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/managing-issues)
* [About Labels](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels)
* [About Milestones](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/about-milestones)
* [Issue Templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests)
* [Configuring Issue Templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)
* [About Issue Forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms)
* [Linking a Pull Request to an Issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue)
* [GitHub Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
* [GitHub CLI Issue Commands](https://cli.github.com/manual/gh_issue)
